# Docker_Images_forWin10Home
Images that downloaded from Docker Hub Operation.

Implementation
==============================
* Commands related Docker 
``` 
docker create イメージ名
docker start コンテナ名
docker run イメージ名
docker stop コンテナ名
docker rm コンテナ名
docker ps
docker exec コンテナ名 実行コマンド
docker rename 古いコンテナ名 新しいコンテナ名
docker cp コピーファイル コピー先
docker diff コンテナ名
docker commit コンテナ名 ユーザ名/イメージ:タグ名
docker export/import コンテナ名/ファイル名
docker save/load ファイル名 イメージ名
docker stats コンテナ名
docker restart コンテナ名
docker pause/unpause コンテナ名
docker attach コンテナ名
docker top コンテナ名
docker port コンテナ名
docker version
docker info
``` 

* docker create 
--name for later container's name
``` 
>docker images
REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
dorowu/ubuntu-desktop-lxde-vnc   latest              68b31e964e2f        2 months ago        1.21GB
docker4w/nsenter-dockerd         latest              2f1c802f322f        4 months ago        187kB

>docker create --name "test01" dorowu/ubuntu-desktop-lxde-vnc
698636288ffd8fad8e4c651c34201d724006320e755cd8d1cc44107cbeb9b42a

>docker ps -a
CONTAINER ID        IMAGE                            COMMAND             CREATED             STATUS              PORTS               NAMES
698636288ffd        dorowu/ubuntu-desktop-lxde-vnc   "/startup.sh"       10 seconds ago      Created                                 test01
``` 

* docker start 
``` 
>docker start test01
test01

>docker ps -a
CONTAINER ID        IMAGE                            COMMAND             CREATED             STATUS                            PORTS               NAMES
698636288ffd        dorowu/ubuntu-desktop-lxde-vnc   "/startup.sh"       5 minutes ago       Up 7 seconds (health: starting)   80/tcp              test01
``` 


* docker run

-i:standard output、
-t:tty
![alt tag](https://i.imgur.com/JmS2W4E.jpg)

-p: port 80 of container mapping to port 8080 of localhost 
![alt tag](https://i.imgur.com/oO4k8Ko.jpg)

``` 
>docker run -c=512 -m=512m -v=C:\Users\user\test01:/var/log --name "test01" ubuntu
``` 

* docker stop

``` 
>docker stop test01
test01

>docker ps -a
CONTAINER ID        IMAGE                            COMMAND              CREATED             STATUS                         PORTS               NAMES
698636288ffd        dorowu/ubuntu-desktop-lxde-vnc   "/startup.sh"        11 minutes ago         Exited (0) About an hour ago                       test01
``` 

* docker rm
``` 
>docker rm test01
test01
``` 

* docker ps
-a: Also show stopped container
``` 
r>docker ps -a
CONTAINER ID        IMAGE                            COMMAND              CREATED             STATUS                         PORTS               NAMES
9705b6b9261e        ubuntu                           "/bin/bash"          23 minutes ago      Exited (127) 16 minutes ago                        test03
698636288ffd        dorowu/ubuntu-desktop-lxde-vnc   "/startup.sh"        3 hours ago         Exited (0) About an hour ago                       test01
``` 

* docker exec 
Execute CLI on container
![alt tag](https://i.imgur.com/HOnK0EF.jpg)

``` 
>docker top test03
PID                 USER                TIME                COMMAND
9334                root                0:00                /bin/bash
9400                root                0:00                /bin/bash
``` 

* docker cp
```
>docker cp test01:/usr/local/apache2/logs/httpd.pid C:\Users\user\Documents
```

```
>docker cp C:\Users\user\Documents\mod_httpd.pid test01:/usr/local/apache2/logs/
```

* docker diff
Container vs Original Image
```
>docker diff test01
C /root
A /root/.bash_history
C /usr/local/apache2/logs
A /usr/local/apache2/logs/httpd.pid
A /usr/local/apache2/logs/mod_httpd.pid
```

* docker commit 
```
>docker commit test01 user/test1:0.1
sha256:23c46ec8cb654a9f1e9104e2254d7d3b9ff147e7836c46acad620aaecc571bb8

>docker images
REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
user/test1                       0.1                 23c46ec8cb65        11 seconds ago      1.21GB
ubuntu                           latest              47b19964fb50        11 days ago         88.1MB
```

* docker export/import name of a container/name of a file

A container export to a file, and then import the file to recover to images
```
>docker export test01 > test01.tar
```

```
>docker import test01.tar
sha256:021d244b9b95f8acbaf357887667b8e86df94085af1140baaa6ec037f94abd3a

>docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
<none>                      <none>              021d244b9b95        6 seconds ago       174MB
```

* docker save/load name of a file; name of a image

Images saved to files; recovery from files to images
```
>docker save -o httpdsave.tar httpd
```

```
>docker load -i httpdsave.tar
540a7775e563: Loading layer [==================================================>]  3.584kB/3.584kB
f57aec6a98ba: Loading layer [==================================================>]   2.56kB/2.56kB
7e41cdcec3c2: Loading layer [==================================================>]   5.12kB/5.12kB
ab31df24cf72: Loading layer [==================================================>]  46.59MB/46.59MB
c7f490f23a8d: Loading layer [==================================================>]  10.29MB/10.29MB
25b1f3c8c9ad: Loading layer [==================================================>]  3.584kB/3.584kB
Loaded image: httpd:latest
```

# Other commands
```
> docker run -e env_varable=value
> docker run -w directory
```

```
> docker stats test01

CONTAINER           CPU %               MEM USAGE / LIMIT    MEM %               NET I/O             BLOCK I/O           PIDS
test01              0.01%               5.48MiB / 973.7MiB   0.56%               828B / 0B           2.92MB / 0B         82
```

```
> docker restart test03
```

```
> docker pause test03

> docker unpause test03
```

```
> docker attach test03
```

```
> docker top test03
```

```
> docker port test03
```

```
> docker version
```

```
> docker info
```

Environment
==============================
``` 
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [Docker for Windowsでイメージからコンテナを生成／操作してみる](https://qiita.com/fkooo/items/ad7d023b59df71cc9a60)


* []()
![alt tag]()