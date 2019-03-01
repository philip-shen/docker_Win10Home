# Docker Network Introduction
> Ethernet or WLAN, Hyper-V Virtual Ethernet Adapter, Docker NAT and various Docker Network types Relationship

# (Physical/Logic) Network Interface Environment

## Network Interfaces(NIC) Configuration on Laptop
![alt tag](https://i.imgur.com/3KpLxPm.jpg)
Hyper-V Virtual Ethernet Adapter#2 for Docker Desktop

## IP Address of each Network Interfaces(NIC)
![alt tag](https://i.imgur.com/Eh5WYF3.jpg)

## MAC Address of each Network Interfaces(NIC)
![alt tag](https://i.imgur.com/eS1xxt1.jpg)

## Route Print
![alt tag](https://i.imgur.com/dwvP7Ei.jpg)

####  Red Block shows default route
####  Organe Block shows 10.0.75.0/24 route
####  Blue  Block shows 10.0.75.0/32 route
####  Green  Block shows 192.168.1.0/24 route

## Tracert to test
![alt tag](https://i.imgur.com/FFyvCDt.jpg)

# Docker Network Types

## Default Networks
``` 
D:\project\docker\docker_Win10Home (master -> origin)
λ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
6729648151ec        bridge              bridge              local
0f11202c8d21        host                host                local
7a14969be2b8        none                null                local
``` 

## none Newtork

![alt tag](https://i.imgur.com/oNLtQwq.jpg)

``` 
λ docker run --name none_net_busybox --net=none -ti busybox /bin/sh
``` 

``` 
D:\project\docker\docker_Win10Home (master -> origin)
λ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
cd2cbf4ebe65        busybox             "/bin/sh"           15 hours ago        Up About a minute                       none_net_busybox

D:\project\docker\docker_Win10Home (master -> origin)
λ docker exec -it none_net_busybox /bin/sh
/ # cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
/ # ifconfig
lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
``` 

## host Newtork

![alt tag](https://i.imgur.com/lMqQrHB.jpg)

``` 
λ docker run --name host_net_busybox --net=host -ti busybox /bin/sh
``` 

``` 
D:\project\docker\docker_Win10Home (master -> origin)
λ docker exec -it host_net_busybox /bin/sh
/ # ifconfig
docker0   Link encap:Ethernet  HWaddr 02:42:58:85:51:90
          inet addr:172.17.0.1  Bcast:172.17.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:58ff:fe85:5190/64 Scope:Link
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:5 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:438 (438.0 B)

eth0      Link encap:Ethernet  HWaddr 02:50:00:00:00:01
          inet addr:192.168.65.3  Bcast:192.168.65.255  Mask:255.255.255.0
          inet6 addr: fe80::50:ff:fe00:1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:55 errors:0 dropped:0 overruns:0 frame:0
          TX packets:88 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:15156 (14.8 KiB)  TX bytes:6633 (6.4 KiB)

hvint0    Link encap:Ethernet  HWaddr 00:15:5D:7B:35:28
          inet addr:10.0.75.2  Bcast:0.0.0.0  Mask:255.255.255.0
          inet6 addr: fe80::215:5dff:fe7b:3528/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:907 errors:0 dropped:0 overruns:0 frame:0
          TX packets:928 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:94969 (92.7 KiB)  TX bytes:75516 (73.7 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:2 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:140 (140.0 B)  TX bytes:140 (140.0 B)
``` 

``` 
/ # iproute list
default via 192.168.65.1 dev eth0
10.0.75.0/24 dev hvint0 scope link  src 10.0.75.2
127.0.0.0/8 dev lo scope host
172.17.0.0/16 dev docker0 scope link  src 172.17.0.1
192.168.65.0/24 dev eth0 scope link  src 192.168.65.3
``` 

### Traceroute to test
#### Cause default route via 192.168.65.1 dev eth0 issue -I
####  -I      Use ICMP ECHO instead of UDP datagrams

``` 
/ # traceroute -I -v 192.168.1.1
traceroute to 192.168.1.1 (192.168.1.1), 30 hops max, 46 byte packets
 1  ZyXEL.Home (192.168.1.1) 26 bytes to (null)  17.177 ms  3.453 ms  6.267 ms
/ # traceroute -I -v 192.168.1.104
traceroute to 192.168.1.104 (192.168.1.104), 30 hops max, 46 byte packets
 1  LAPTOP-DDB6QQED.Home (192.168.1.104) 26 bytes to (null)  8.347 ms  1.761 ms  1.197 ms
``` 

## bridge Newtork

![alt tag](https://i.imgur.com/7bWl90c.jpg)
Linux bridge で仮想インタフェースを作成し、そのインタフェースに対してveth でDocker コンテナと接続する方式で、Docker ホストが属するネットワークとは異なる、仮想bridge 上のネットワークにコンテナを作成し、NAT 形式で外部のノードと通信する形式です。
Docker のコンテナ作成時に--net オプションを特に何も指定しない場合は、自動的にこれが選択されたものとしてDocker は振る舞います。

``` 
λ docker run --name bridge_net_busybox -ti busybox /bin/sh
/ # ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:758 (758.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ # exit
``` 

``` 
λ ipconfig

Windows IP Configuration


Ethernet adapter vEthernet (DockerNAT):

   Connection-specific DNS Suffix  . :
   IPv4 Address. . . . . . . . . . . : 10.0.75.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :
``` 
## About Docker Network (communciations between containers)
![alt tag](https://camo.qiitausercontent.com/b4a2df6d7b8d9584b6fdf402b6406e5e8fdcd5ae/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3230393930392f63323961353535302d333930372d636336372d646434652d3034656537666537306662322e706e67)
* [【図解】Dockerの全体像を理解する -中編-](https://qiita.com/kotaro-dr/items/88ec3a0e2d80d7cdf87a)

## overlay Newtork

![alt tag](https://i.imgur.com/JGWMlqV.jpg)

Docker overlay network を使うことで異なるL3 上(Docker の場合は異なるDocker ホスト上) に存在するコンテナに対して、同じネットワークに存在するコンテナとして透過的にアクセスすることができるようになります。
なお、このDocker overlay network はVXLAN を使って実装されているため、Docker ホストが異なる拠点やデータセンタに存在しても、そのDocker ホスト上のコンテナは、同じネットワーク上に存在しているものとしてアクセスすることができるようになります。
既にDocker には--link 機能がありますが、それは異なるDocker ホストをまたがって名前解決し、アクセスすることは不可能でした。

# Troubleshooting


Environment
==============================
``` 
o ASUS VivoBook S14, Core-i3 8th, RAM:4GB。
o WSL on Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [Docker network 概論 2016-02-08](https://qiita.com/TsutomuNakamura/items/ed046ee21caca4a2ffd9)

* [How To Create A Transparent Network For Windows Containers March 30, 2017](https://www.ntweekly.com/2017/03/30/how-to-create-a-transparent-network-with-windows-containers/)

* [【図解】Dockerの全体像を理解する -中編-](https://qiita.com/kotaro-dr/items/88ec3a0e2d80d7cdf87a)
* [【図解】Dockerの全体像を理解する -後編-](https://qiita.com/kotaro-dr/items/40106f13d47bfcbc2572)

* [docker buildする際にhost側のssh keyを使ってbuildする](https://qiita.com/toyama0919/items/190eb19298e523094ba2)
* [DockerコンテナのSSL化](https://qiita.com/KingSky/items/c407829991cb18a52d99)
* [DockerでSSH keyを使ってbitbucketの非公開レポジトリをクローン](https://qiita.com/jerrywdlee/items/38d5442cd5677dbad79f)
* [SSH サーバープロセスを実行する DockerイメージをMacで作成](https://qiita.com/tonishy/items/373481114887b369aeba)

* [WSLでDockerを動かす](https://qiita.com/sugimount/items/b6277ca4a97b4a05ac11)
* [Windows10 HomeでDocker環境を構築](https://qiita.com/shio1997/items/8ea3de0c7364526718b0)
* [Dockerで開発用デスクトップ環境（GUI）を構築する XRDP](https://qiita.com/kanedaq/items/3e3dc2181e1cfd1da1d6)

* [DockerコンテナでPythonスクリプトを動かす](https://qiita.com/zaki-lknr/items/f0ca0a28e5445884f30a)
* [Docker for Windowsが起動してくれないので、処方箋を調べて色々試してみた Hyper-V](https://qiita.com/kurone-kito/items/2e41a58f01b3d3620109)
* [Docker を使う（python のイメージで色々確認してみる）](https://qiita.com/landwarrior/items/fd918da9ebae20486b81)
* [Dockerのコマンドが長いのでalias設定した](https://qiita.com/kulikala/items/f736629497a974ca82cb)
* [Dockerイメージ、コンテナの場所](https://www.neko6.info/archives/1979)

* [Docker マルチホストネットワーキング: swarm, consul, overlay network(VXLAN) を使用した構築手順 2016-03-28](https://qiita.com/TsutomuNakamura/items/2dfa428adb6ba3983525)
* [dockerのlinkオプションがレガシーなので、コンテナ間で名前解決できるようにネットワークを用意する 2017-05-08](https://qiita.com/tamanobi/items/8b8dd64ae1f959f9ff9f)
* [Dockerのネットワークをbridgeで外と繋げる 2016-03-26](https://qiita.com/kjtanaka/items/c2fd862ca91a63d74166)
* [Assign static IP to Docker container Jan 14 '15](https://stackoverflow.com/questions/27937185/assign-static-ip-to-docker-container)
* [記録:Dockerコンテナ間通信(bridge) 2018-10-24](https://qiita.com/mincoshi/items/5a1542b523195a9c11ed)
* [Multi-Host Docker Networking is now ready for production November 3, 2015](https://blog.docker.com/2015/11/docker-multi-host-networking-ga/)
* [Dockerを知らなくてもDockerコンテナでHadoop環境を構築する 2019-01-04](https://qiita.com/kuromt_/items/fbe04c04a97a996490ec)
* [マルチホストDockerネットワーキング（正式版 native overlay）2015-11-13](https://qiita.com/nmatsui/items/2811f2297343e1d50739)

* [Hyper-Vのネットワーク設定についての覚え書き 2016-12-24](https://qiita.com/kit/items/56916c4285f353e89ea0)
* [Hyper-Vを一時的にON/OFF切り替える方法（VMWareと共存させたいときとか） 2014-04-07](https://blog.okazuki.jp/entry/2014/04/07/100738)
* [Hyper-Vネットワークの基本](http://www.vwnet.jp/Windows/WS12R2/Hyper-V/Hyper-V_Network.htm)

* []()
![alt tag]()