# Purpose
Samba usage on Docker at Windows  

# Table of Contents  
[Docker for Windowsのネットワーク構成](#docker-for-windows%E3%81%AE%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E6%A7%8B%E6%88%90)  
[docker volumeの作成](#docker-volume%E3%81%AE%E4%BD%9C%E6%88%90)  
[sambaコンテナの作成](#samba%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%81%AE%E4%BD%9C%E6%88%90)  
[routeの設定](#route%E3%81%AE%E8%A8%AD%E5%AE%9A)  
[補足](#%E8%A3%9C%E8%B6%B3)  

[Reference](#reference)

[Docker for Windowsでsambaを使う Apr 11, 2018](https://qiita.com/KNaito/items/0d67fc2293e15c3960fc)  
```
そこで、この記事では、docker volumeをsambaでマウントして、
Windowsから編集出来るようにする方法を紹介します。

(なお、ここに紹介する方法を使わなくても、ローカルでsambaのポート(139や445など)を
使用していない場合は、単純にローカルのポートフォワードでsambaサーバーに接続することが
出来るかも知れません。私の環境では、これらのポートをWindows側で使用しているためダメでした。)
```

# Docker for Windowsのネットワーク構成  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F105847%2Fd9de520d-64e4-4b0d-2429-2aaf0881f42a.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&w=1400&fit=max&s=36e7052201fd3fe8b8169a77d57009e7)

・Windowsホストとコンテナの間には、MobyLinuxVMがあります。
・WindowsホストとMobyLinuxVMはDockerNATというネットワークでつながっています。
・MobyLinuxVMと各コンテナはDocker0というネットワークでつながっています。(コンテナをデフォルトで立ち上げた場合）

このままでは、Windowsホストからコンテナ1のSambaに接続出来ないため、routeコマンドでDocker0ネットワークへのルートを設定してあげるというのがこの記事の趣旨です。

# docker volumeの作成  
```
> docker volume create samba-public
```

# sambaコンテナの作成  
ここでは簡単に、既存のsambaコンテナのイメージである dperson/sambaを使用して、sambaコンテナを立ち上げます。dperson/sambaについて詳しくは
https://hub.docker.com/r/dperson/samba/~/dockerfile/
を確認してください。

マウント先ディレクトリは/mnt/publicとし、ログイン不要で誰でも読み書き出来るものとします。共有フォルダ名はpublicです。また、コンテナ名としてsamba_serverとしました。
```
> docker run --name samba_server -v samba-public:/mnt/public -d dperson/samba -s "public;/mnt/public;yes;no;yes;all;none"
```

これでsambaコンテナが立ち上がりました。このコンテナはdockerのブリッジネットワーク(172.17.x.x)で立ち上がっていると思います。次のコマンドでIPアドレスを確認してみてください。
```
> docker exec -it samba_server ip addr show dev eth0
```

私の環境では、次のような結果になりました。
```
9: eth0@if10: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```
つまり、このコンテナのIPアドレス/サブネットは、172.17.0.2/16です。つまり、ネットワークとしては、172.17.0.0/255.255.0.0 となります。

# routeの設定  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F105847%2F45905dba-c30b-2236-f995-15241c55abd7.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&s=f2ec360af61590e7fdc425312e605348)  

![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F105847%2Fd5ec99b5-5351-4a66-b347-fba5a3d2c5fd.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&s=ab5600604abc2485c8c3a421e952d904)  

```
> arp -a
インターフェイス： 10.0.75.1 --- 0x20
 インターネット アドレス　物理アドレス　種類
 10.0.75.2    00-15-5d-02-81-1d   動的
 10.1.75.255  ff-ff-ff-ff-ff-ff   静的
  ...
```

```
> route add 172.17.0.0 mask 255.255.0.0 10.0.75.2
```

```
\\172.17.0.2
```
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F105847%2F573b86d3-4ab9-b53a-3985-6ce5631bda87.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&s=daf39d6f337f6204ea1191d2d0405398)  

# 補足  
route addコマンドは、Windowsをリブートすると消えてしまうので、永続化したい場合は、-pオプションを使います。  
```
> route -p add 172.17.0.0 mask 255.255.0.0 10.0.75.2
```

```
> route delete 172.17.0.0
```

```
> route print
```

# Troubleshooting


# Reference  
* [Docker を Linux 以外で使うことは幸せにならない](https://qiita.com/il-m-yamagishi/items/1d2344a32744f56a8cc6)
[Docker for Windows](https://qiita.com/il-m-yamagishi/items/1d2344a32744f56a8cc6#docker-for-windows)  
Hyper-V 上で動かす...

  Samba IO やっぱり遅い
      [Shared Volumes Slow · Issue #188 · docker/for-win](https://github.com/docker/for-win/issues/188)  
  NAT の調子が悪くなることがあるという話


```

```


```
```
* []()
![alt tag]()  

# h1 size

## h2 size

### h3 size

#### h4 size

##### h5 size

*strong*strong  
**strong**strong  

> quote  
> quote

- [ ] checklist1
- [x] checklist2

* 1
* 2
* 3

- 1
- 2
- 3
