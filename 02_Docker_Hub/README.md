# Docker_Hub_Win10Home
Download or Upload images from or to Docker Hub.

Implementation
==============================
* Commands related Docker Hub
``` 
docker pull イメージ名:タグ名
docker images
docker inspect イメージ名
docker tag 元のイメージ名:タグ名 ユーザ名/イメージ名:タグ名
docker search イメージ名
docker rmi イメージ名
docker login
docker push ユーザ名/イメージ名:タグ名
docker logout
``` 

* docker search 
``` 
>docker search ubuntu
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
ubuntu                                                 Ubuntu is a Debian-based Linux operating sys…   9198                [OK]
dorowu/ubuntu-desktop-lxde-vnc                         Ubuntu with openssh-server and NoVNC            270                                     [OK]
rastasheep/ubuntu-sshd                                 Dockerized SSH service, built on top of offi…   201                                     [OK]
``` 

* Attenation:
While error messages appears...
``` 
>docker search ubuntu
Error response from daemon: Get https://index.docker.io/v1/search?q=ubuntu&n=25: dial tcp: lookup index.docker.io on 192.168.65.1:53: read udp 192.168.65.3:49921->192.168.65.1:53: i/o timeout
``` 
Modify DNS setting to fixed.
![alt tag](https://camo.qiitausercontent.com/1345bbeb77a8b193b6a04085822b0b224411d5d2/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3138383534342f31316564643966382d643932652d313830612d333562312d3162616531636563303963642e706e67)

* docker pull 
![alt tag](https://i.imgur.com/lyBE9gb.jpg)

* docker images
``` 
>docker images
REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
dorowu/ubuntu-desktop-lxde-vnc   latest              68b31e964e2f        2 months ago        1.21GB
docker4w/nsenter-dockerd         latest              2f1c802f322f        4 months ago        187kB
``` 

* docker inspect
``` 
>docker inspect 68b31e964e2f
[
    {
～～～～～～
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 1209311875,
        "VirtualSize": 1209311875,
～～～～～～        
``` 

* docker login

* docker push 

* docker logout

Environment
==============================
``` 
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [WindowsでDocker Hubからイメージをダウンロードしたりアップロードしたりしてみる](https://qiita.com/fkooo/items/8a29e308f9eb9ce7648e)


* []()
![alt tag]()