# Docker Machine


What is Docker MachineDocker Machine?
==============================
ホストマシンにDockerの実行環境をコマンドで構築できる（Docker Engine をインストールしてDockerマシンを作ることができる）ツール

構築環境は手元のWindows、Mac等に限定されずクラウド環境にも構築できる

クラウド環境はAmazon Web Services/Microsoft Azure/Google Compute Engine等々をサポート

Docker Hubと連携して、自分の手元のマシンで開発環境のDockerイメージを作成、Docker Hubに保存、Docker Machineで構築したAWSのDocker実行環境に同じ開発環境を作成する。
等ができるようになる ⇒つまりDockerを複数のマシン／クラウドといったマルチホスト環境で運用できるようになる

* [公式サイト Docker Machine](http://docs.docker.jp/machine/)
* [公式サイト サポート環境](http://docs.docker.jp/machine/drivers/)

# Commands related Docker 
``` 
docker-machine create -d ドライバ名 作成するマシン名
docker-machine ls
docker-machine ssh マシン名
docker-machine scp マシン名:ダウンロードファイル 格納フォルダ
docker-machine ip マシン名
docker-machine inspect マシン名
docker-machine env マシン名
docker-machine start マシン名
docker-machine stop マシン名
docker-machine kill マシン名
docker-machine rm マシン名
``` 

# Preparation

初期状態のHyper-Vでは、仮想スイッチがDockerNATしかないためIPアドレスを使った接続ができない。必要であれば、Hyper-Vの仮想スイッチを追加する

## Add Virutal Switch procedure

Start Hyper-V machine, 
![alt tag](https://i.imgur.com/idUfPRQ.jpg)

And then select Virtual Switch Management.
![alt tag](https://i.imgur.com/pVObVUT.jpg)

Select "New virtual network switch" and click "Create Virutal Swtich".
![alt tag](https://i.imgur.com/va36uH4.jpg)

Rename virutal switch, select a netwrok card in external network、enable "Allow management..." and click "OK".
Restart PC
![alt tag](https://i.imgur.com/1F6Wq39.jpg)

* [Microsoft Hyper-V in Docker Docs](https://docs.docker.com/machine/drivers/hyper-v/)

## docker-machine create -d 
Creat VM on Hyper-V, boot2docker image installation、SSH、Virtual Swtich、Virtual Swtich setup
``` 
>docker-machine create -d hyperv --hyperv-virtual-switch "VirtualSW" --hyperv-memory "306" --hyperv-disk-size "5000" machine
Running pre-create checks...
Creating machine...
(machine) Copying C:\Users\SCS\.docker\machine\cache\boot2docker.iso to C:\Users\SCS\.docker\machine\machines\machine\boot2docker.iso...
(machine) Creating SSH key...
(machine) Creating VM...
(machine) Using switch "VirtualSW"
(machine) Creating VHD
(machine) Starting VM...
(machine) Waiting for host to start...
``` 

Other option:
![alt tag](https://camo.qiitausercontent.com/81d10a92968000b08760c2f1bf6218f1f4e7b200/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3138383534342f64313534663533372d316537312d313566632d623730322d3237363632363566363830302e706e67)


## docker-machine ls
Docker Machineで管理しているマシンの一覧／状態を表示するコマンド

```
> docker-machine ls
NAME      ACTIVE   DRIVER   STATE     URL                       SWARM   DOCKER        ERRORS
machine   -        hyperv   Running   tcp://192.168.0.XX:2376           v17.12.0-ce
```



## docker-machine ssh マシン名

Docker Machineで作成したマシンにSSHで接続するコマンド
```
> docker-machine ssh machine
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 17.12.0-ce, build HEAD : 378b049 - Wed Dec 20 23:39:20 UTC 2017
Docker version 17.12.0-ce, build c97c6d6
```


## docker-machine scp マシン名:ダウンロードファイル 格納フォルダ

Docker Machineで作成したマシンからSCPでファイルをダウンロードするコマンド

```
> docker-machine scp machine:/home/docker/log.log .
You must have a copy of the scp binary locally to use the scp feature
```
ローカル環境にscpがなかったためエラーになったが、できるはず^^;

## docker-machine ip

Docker Machineで作成したマシンのIPアドレスを確認するコマンド
```
> docker-machine ip machine
192.168.0.XX
```

## docker-machine inspect

Docker Machineで作成したマシンのCPUやメモリなど構成の詳細を確認するコマンド
構成情報がJSON形式で返却される
```
> docker-machine inspect machine
{
    "ConfigVersion": 3,
    "Driver": {
        "IPAddress": "192.168.0.XX",
        "MachineName": "machine",
        "SSHUser": "docker",
        "SSHPort": 22,
～～～省略～～～
        "VSwitch": "virtualSW",
        "DiskSize": 20000,
        "MemSize": 1024,
        "CPU": 1,
        "MacAddr": "",
        "VLanID": 0
    },
～～～省略～～～
```

## docker-machine env

通常、Dockerコマンド実行にはDockerマシンにSSHで接続後、Dockerマシン上でコマンド実行する必要がある。このコマンドは、SSH接続不要でホストマシン上で特定のDockerマシンに直接コマンド実行するために環境変数を設定するコマンド
docker-machine env と & C: から始まるコマンド２つを実行する必要がある
```
> docker-machine env machine
$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://192.168.0.XX:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\user\.docker\machine\machines\machine"
$Env:DOCKER_MACHINE_NAME = "machine"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env machine | Invoke-Expression

> & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env machine | Invoke-Expression
```

Dockerマシン上じゃなくても直接コマンド実行できるようになる
```
> docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
50aff78429b1: Pull complete
f6d82e297bce: Pull complete
275abb2c8a6f: Pull complete
9f15a39356d6: Pull complete
fc0342a94c89: Pull complete
Digest: sha256:ec0e4e8bf2c1178e025099eed57c566959bb408c6b478c284c1683bc4298b683
Status: Downloaded newer image for ubuntu:latest

> docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              00fd29ccc6f1        2 weeks ago         111MB
```

戻すときは-uを指定する
```
PS C:\WINDOWS\system32> docker-machine env -u
Remove-Item Env:\\DOCKER_TLS_VERIFY
Remove-Item Env:\\DOCKER_HOST
Remove-Item Env:\\DOCKER_CERT_PATH
Remove-Item Env:\\DOCKER_MACHINE_NAME
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env -u | Invoke-Expression

> & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env -u | Invoke-Expression
```

## docker-machine start 

マシンの起動コマンド
最初はdocker-machine createでマシンがそのまま起動するが、stop/killで停止した場合に使用する
```
> docker-machine start machine
Starting "machine"...
(machine) Waiting for host to start...
Machine "machine" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
```

## docker-machine stop

マシンの停止コマンド
```
> docker-machine stop machine
Stopping "machine"...
(machine) Waiting for host to stop...
Machine "machine" was stopped.
```

## docker-machine kill

マシンの停止コマンド
stopで停止できない場合に使用する
```
> docker-machine kill machine
Killing "machine"...
(machine) Waiting for host to stop...
Machine "machine" was killed.
```

## docker-machine rm 

マシンの削除コマンド
削除確認があるので y を入力
```
> docker-machine rm machine
About to remove machine
WARNING: This action will delete both local reference and remote instance.
Are you sure? (y/n): y
(machine) Waiting for host to stop...
Successfully removed machine
```

# Troubleshooting

## Error creating machine: Error in driver during machine creation: exit status 1
> Running docker-machine -D start vmname show you the error. Mine was lack of memory... Added RAM to host VM and everything ran fine after...

* [Hyperv/Windows10 - Error creating machine: Error in driver during machine creation: exit status 1](https://github.com/docker/machine/issues/2267)

## Error getting IP address: Something went wrong running an SSH command! command
> While Starting docker terminal below error populating, how to fix this Error getting IP address: Something went wrong running an SSH command!

* [Error getting IP address: Something went wrong running an SSH command! command](https://stackoverflow.com/questions/34651909/error-getting-ip-address-something-went-wrong-running-an-ssh-command-command)

Environment
==============================
``` 
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [WindowsでDocker Machineを試してみる](https://qiita.com/fkooo/items/356c9954181c00818f12)
* [公式サイト Docker Machine](http://docs.docker.jp/machine/)
* [公式サイト サポート環境](http://docs.docker.jp/machine/drivers/)
* [Docker Machine 入門(Hyper-Vの場合)](https://qiita.com/ohhara_shiojiri/items/801a24a2c94ff93be82d)
* [Docker Machine Windows 10 Hyper-V Troubleshooting Tips](https://rominirani.com/docker-machine-windows-10-hyper-v-troubleshooting-tips-367c1ea73c24)

* []()
![alt tag]()