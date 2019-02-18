# DockerFile
Environment definition inside your container for Docker.

Implementation
==============================
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



Environment
==============================
``` 
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [Windowsで構成情報をDockerfileに定義してイメージを作成してみる](https://qiita.com/fkooo/items/53b2ea865e8c2c7fec27)
* [Dockerfileを極めて、Dockerマスターになろう！](https://qiita.com/soushiy/items/0945bcbc7ecce4822985)
* [Dockerfile の基本と、CMD と ENTRYPOINT の違いを理解します。ハンズオン（Dockerfile活用編）](https://qiita.com/zembutsu/items/d146295cfcf69c205c1e#%E3%83%8F%E3%83%B3%E3%82%BA%E3%82%AA%E3%83%B3dockerfile%E6%B4%BB%E7%94%A8%E7%B7%A8)

* []()
![alt tag]()