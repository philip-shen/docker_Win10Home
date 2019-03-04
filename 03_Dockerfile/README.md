# DockerFile
Environment definitions inside your container for Docker.

## To activate speific containers from spefic images by Dockerfiles
![alt tag](https://camo.qiitausercontent.com/7de62ce86f2eba6862f8ca689603741a57d5bfc6/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3230393930392f37646130636238362d653330352d376663302d316665312d6437626534643836666662612e706e67)

# Implementation

* Execute command
``` 
docker build -t image Dockerfile folder
docker history image
``` 

* Handon of Dockerfile
``` 
d:\project\docker\docker_Win10Home>mkdir myping

d:\project\docker\docker_Win10Home>cd myping
``` 

Create a Dockerfile
``` 
d:\project\docker\docker_Win10Home\myping>type Dockerfile
FROM busybox
CMD ["ping","-c","10","127.0.0.1"]
``` 

``` 
d:\project\docker\docker_Win10Home\myping>docker build -t myping .
Sending build context to Docker daemon   2.56kB
Step 1/2 : FROM busybox
latest: Pulling from library/busybox
697743189b6d: Pull complete
Digest: sha256:061ca9704a714ee3e8b80523ec720c64f6209ad3f97c0ff7cb9ec7d19f15149f
Status: Downloaded newer image for busybox:latest
 ---> d8233ab899d4
Step 2/2 : CMD ["ping","-c","10","127.0.0.1"]
 ---> Running in c7a37d108a16
Removing intermediate container c7a37d108a16
 ---> 19c632ef0eb5
Successfully built 19c632ef0eb5
Successfully tagged myping:latest
``` 

``` 
d:\project\docker\docker_Win10Home\myping>docker images
REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
myping                           latest              19c632ef0eb5        2 minutes ago       1.2MB
``` 

``` 
d:\project\docker\docker_Win10Home\myping>docker run --rm myping
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=64 time=0.048 ms
64 bytes from 127.0.0.1: seq=1 ttl=64 time=0.123 ms
64 bytes from 127.0.0.1: seq=2 ttl=64 time=0.134 ms
64 bytes from 127.0.0.1: seq=3 ttl=64 time=0.109 ms
64 bytes from 127.0.0.1: seq=4 ttl=64 time=0.112 ms
64 bytes from 127.0.0.1: seq=5 ttl=64 time=0.107 ms
64 bytes from 127.0.0.1: seq=6 ttl=64 time=0.110 ms
64 bytes from 127.0.0.1: seq=7 ttl=64 time=0.103 ms
64 bytes from 127.0.0.1: seq=8 ttl=64 time=0.060 ms
64 bytes from 127.0.0.1: seq=9 ttl=64 time=0.072 ms

--- 127.0.0.1 ping statistics ---
10 packets transmitted, 10 packets received, 0% packet loss
round-trip min/avg/max = 0.048/0.097/0.134 ms
``` 

Add option
``` 
d:\project\docker\docker_Win10Home\myping>docker run -it myping sh
/ # ls
bin   dev   etc   home  proc  root  sys   tmp   usr   var
/ # exit

d:\project\docker\docker_Win10Home\myping>
``` 
If add option, igoner CMD arguments.

```
Ubuntu イメージの Dockerfile
    CMD [“/bin/bash”]
Busybox イメージの Dockerifle
    CMD [“sh”]
```

* ENTRYPOINT vs CMD
Update content of Dockfile
```
d:\project\docker\docker_Win10Home>type .\myping\Dockerfile
FROM busybox
ENTRYPOINT ["ping"]
```

Next
```
d:\project\docker\docker_Win10Home\myping>docker build -t myping:1.1 .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM busybox
 ---> d8233ab899d4
Step 2/2 : ENTRYPOINT ["ping"]
 ---> Running in 0cfb1439470d
Removing intermediate container 0cfb1439470d
 ---> 6865582bf293
Successfully built 6865582bf293
Successfully tagged myping:1.1
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.

d:\project\docker\docker_Win10Home\myping>docker images
REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
myping                           1.1                 6865582bf293        12 seconds ago      1.2MB
myping                           latest              19c632ef0eb5        23 hours ago        1.2MB
```

Next
``` 
d:\project\docker\docker_Win10Home\myping>docker run --rm myping:1.1
BusyBox v1.30.1 (2019-02-14 18:58:02 UTC) multi-call binary.

Usage: ping [OPTIONS] HOST

Send ICMP ECHO_REQUEST packets to network hosts
.......

```

CMD … docker run: CMD ignore if argumenets exist.
ENTRYPOINT … docker run: ENTRYPOINT if argumenets exist.

```
d:\project\docker\docker_Win10Home\myping>docker run --rm myping:1.1 127.0.0.1
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=64 time=0.064 ms
64 bytes from 127.0.0.1: seq=1 ttl=64 time=0.041 ms
64 bytes from 127.0.0.1: seq=2 ttl=64 time=0.043 ms
64 bytes from 127.0.0.1: seq=3 ttl=64 time=0.042 ms
64 bytes from 127.0.0.1: seq=4 ttl=64 time=0.067 ms
```

Update contnet in Dockerfile 
```
d:\project\docker\docker_Win10Home\myping>type Dockerfile
FROM busybox

CMD ["127.0.0.1"]
ENTRYPOINT ["ping"]

d:\project\docker\docker_Win10Home\myping>docker build -t myping .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM busybox
 ---> d8233ab899d4
Step 2/3 : CMD ["127.0.0.1"]
 ---> Running in 6a1a36fc286e
Removing intermediate container 6a1a36fc286e
 ---> 775e571c7ea0
Step 3/3 : ENTRYPOINT ["ping"]
 ---> Running in f6c3ffdf5b11
Removing intermediate container f6c3ffdf5b11
 ---> 05df98138995
Successfully built 05df98138995
Successfully tagged myping:latest
SECURITY WARNING: 
```
Then,
```
d:\project\docker\docker_Win10Home\myping>docker run myping
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=64 time=0.082 ms
64 bytes from 127.0.0.1: seq=1 ttl=64 time=0.152 ms
64 bytes from 127.0.0.1: seq=2 ttl=64 time=0.134 ms
64 bytes from 127.0.0.1: seq=3 ttl=64 time=0.113 ms
64 bytes from 127.0.0.1: seq=4 ttl=64 time=0.121 ms
64 bytes from 127.0.0.1: seq=5 ttl=64 time=0.126 ms
64 bytes from 127.0.0.1: seq=6 ttl=64 time=0.119 ms
```


* Commands related Dockerfile
``` 
FROM 作成元イメージ名
RUN 実行コマンド
CMD 実行コマンド
ENTRYPOINT 実行コマンド
ONBUILD 実行コマンド
ENV 環境変数=値
WORKDIR 作業ディレクトリ
USER ユーザ名
LABEL ラベル名=値
EXPOSE ポート番号
ADD ホストのパス イメージのパス
COPY ホストのパス イメージのパス
VOLUME イメージのマウントポイントパス
``` 

* FROM, RUN, CMD
```
d:\project\docker\docker_Win10Home\myping>type Dockerfile
# base image
FROM centos

# httpd installation
RUN ["yum","install","-y","httpd"]

# httpd execution
CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
```
※In Dockerfile, CMD only one exists and last one is available.
（RUN cloud exist serval lines.）

```
d:\project\docker\docker_Win10Home\myping>docker build -t sample .

Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM centos
latest: Pulling from library/centos
～～～省略～～
Step 2/3 : RUN ["yum","install","-y","httpd"]
 ---> Running in 37180fca0ce0
～～～省略～～
Step 3/3 : CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
 ---> Running in d1b3d37e4de8
～～～省略～～
Successfully built 6395b3df2ed9
Successfully tagged sample:latest
```

```
d:\project\docker\docker_Win10Home\myping>docker run -d -p 80:80 sample
3b23a3c34e75578a0e4fe55225cd00e6766659fcabedb2edf2a787f7c5bee419
```

http://localhost/
![alt tag](https://camo.qiitausercontent.com/b31ec46952a342ce668d5ff2e68b0649717a0300/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3138383534342f65636135353633332d363466652d666462392d653530382d6166383239613439376134652e706e67)

* ENTRYPOINT
ENTRYPOINT命令とCMD命令の違いは、docker runの引数に実行コマンドを指定した場合、Deckerfileに指定しているコマンドをコンテナ作成時に
``` 
 Dockerfile

# ベースイメージ
FROM centos

# httpdのインストール
RUN ["yum","install","-y","httpd"]

# httpd実行
ENTRYPOINT ["/usr/sbin/httpd","-D","FOREGROUND"]

``` 

``` 
> docker run -d -p 80:80 sample mkdir test
> docker run -d -p 80:80 sample2 mkdir test
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                          PORTS               NAMES
ea1423d1cce2        sample2             "/usr/sbin/httpd -..."   3 seconds ago        Exited (1) 1 second ago                             unruffled_yonath
7fe16ea0c839        sample              "mkdir test"             About a minute ago   Exited (0) About a minute ago                       sharp_einstein
``` 

* ONBUILD 
ONBUILD命令は、今までのようなイメージ作成／コンテナ作成時ではなく、ONBUILD命令を指定したイメージをベースに次のイメージを作成する時に実行するコマンドを指定する

Dockerfile1
```  
# ベースイメージ
FROM centos

# httpdのインストール
RUN ["yum","install","-y","httpd"]

# index.htmlの配置
ONBUILD ADD index.html /var/www/html/

# httpd実行
ENTRYPOINT ["/usr/sbin/httpd","-D","FOREGROUND"]

``` 

Dockerfile2
``` 
# ベースイメージ
FROM sample1
``` 

``` 
> docker build -t sample1 -f Dockerfile1 .
> docker build -t sample2 -f Dockerfile2 .
``` 

* ENV 環境変数=値
* WORKDIR 作業ディレクトリ
* USER ユーザ名
* LABEL ラベル名=値
* EXPOSE ポート番号

* ADD ホストのパス イメージのパス
* COPY ホストのパス イメージのパス
COPY命令は単純にファイルをコピーするだけ、
ADD命令はリモートファイルのダウンロードやアーカイブの解凍などの機能をもつ

* VOLUME イメージのマウントポイントパス

Dockerfile
```
# ベースイメージ
FROM centos

# ラベル指定
LABEL LABELDESC="DOCKERFILE TEST."

# 環境変数設定
ENV TESTVAR="TEST VALUE."

# httpdのインストール
RUN ["yum","install","-y","httpd"]

# ユーザの切り替え
RUN ["adduser","testuser"]
USER testuser
RUN ["whoami"]
USER root

# 作業ディレクトリ指定
WORKDIR /var/www/html/
RUN ["ls","-l"]

# ファイル、フォルダの配置
ADD index.html /var/www/html/
RUN ["ls","-l"]
COPY temp /var/www/html/
RUN ["ls","-l"]

# ポート指定
EXPOSE 8080

# ボリューム設定
RUN ["mkdir","/var/log/temp"]
VOLUME /var/log/temp

# httpd実行
ENTRYPOINT ["/usr/sbin/httpd","-D","FOREGROUND"]
``` 

Environment
==============================
``` 
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [Windowsで構成情報をDockerfileに定義してイメージを作成してみる](https://qiita.com/fkooo/items/53b2ea865e8c2c7fec27)
* [【図解】Dockerの全体像を理解する -中編- 2019-01-22](https://qiita.com/kotaro-dr/items/88ec3a0e2d80d7cdf87a)

* [Dockerfileについて](https://qiita.com/tanan/items/e79a5dc1b54ca830ac21)
* [Dockerfileを極めて、Dockerマスターになろう！](https://qiita.com/soushiy/items/0945bcbc7ecce4822985)
* [Dockerfile の基本と、CMD と ENTRYPOINT の違いを理解します。ハンズオン（Dockerfile活用編）](https://qiita.com/zembutsu/items/d146295cfcf69c205c1e#%E3%83%8F%E3%83%B3%E3%82%BA%E3%82%AA%E3%83%B3dockerfile%E6%B4%BB%E7%94%A8%E7%B7%A8)

* []()
![alt tag]()