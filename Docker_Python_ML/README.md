# Machine Learning in Python under Docker


Goal
==============================

Hand on Docker operation, 
Practice Docker to build a image to creat a conatiner by Dockerfile and docker-compose.yaml


# Related packages of python for Machine Learing
``` 
python 3.7.2
numpy
scikit-learn
pandas
matplotlib
pytest
``` 

## Directory as below:
bash
``` 
sample001.
├── Dockerfile
├── docker-compose.yml
├── pip
│   └── requirements.txt
├── src
│   └── hello.py
└── test
``` 

hello.py
``` 
print("hello")
``` 

requirements.txt
``` 
numpy
pandas
matplotlib
scikit-learn
pytest
``` 

# Dockerfil Preparation

Dockerのイメージを作る際に使用する設計書

## Dockerfile

``` 
# pythonの3.7.2-slimをベースにする
FROM python:3.7.2-slim

# ARGで変数を定義
# buildする時に変更可能
# コンテナ内のディレクトを決めておく
ARG root_directory=/host_mnt/d/project/docker/sample001

# python3 pipのインストール
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    # imageのサイズを小さくするためにキャッシュ削除
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    # pipのアップデート
    && pip install --upgrade pip

# Dockerfileを実行したディレクトにあるファイルを全てイメージにコピー
COPY . ${root_directory}/

# 機械学習に使用するライブラリのインストール
WORKDIR ${root_directory}/pip/

RUN pip install -r requirements.txt

# ディレレクトリの移動
WORKDIR ${root_directory}
``` 

To reduce image size:
``` 
"-slim" is usued
Just Run command once
Delete cache
``` 

* [http://docs.docker.jp/engine/userguide/eng-image/dockerfile_best-practice.html#build-cache](http://docs.docker.jp/engine/userguide/eng-image/dockerfile_best-practice.html#build-cache)

## Create image and active container

``` 
D:\project\docker\sample001
λ docker build -t python_image .
``` 
![alt tag](https://i.imgur.com/Ykog674.jpg)
![alt tag](https://i.imgur.com/MCJVV6U.jpg)

### Check image if exists or not?
``` 
d:\project\docker\sample001>docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
python_image               latest              b0a329f1a53d        9 minutes ago       826MB
python                     3.7.2-slim          bc5085cee55b        3 days ago          143MB
``` 

## Excute hello.py
``` 
D:\project\docker\sample001
λ docker run -it --rm --name python_contianer python_image /bin/bash
root@0743ae3ad2ce:/host_mnt/d/project/docker/sample001/pip# cd ..
root@0743ae3ad2ce:/host_mnt/d/project/docker/sample001# python src/hello.py
hello
root@0743ae3ad2ce:/host_mnt/d/project/docker/sample001#
``` 

### Check container if active
``` 
C:\Windows\system32>docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c9cabcd8e2ca        python_image        "/bin/bash"         2 minutes ago       Up 2 minutes                            python_container
``` 

### Update hello.py
``` 
print("hello")
print("world")
``` 

### Excute hello.py again
``` 
root@c9cabcd8e2ca:/D:\project\docker\sample001#python src/hello.py
hello
root@c9cabcd8e2ca:/D:\project\docker\sample001#python src/hello.py
hello
``` 
### It seems something wrong...

## Assing Volume and sync folder immediately

### Dockerfile
```
# Dockerfileを実行したディレクトにあるファイルを全てイメージにコピー
COPY . ${root_directory}/
```
while building image, file directory is included in images.

```
$ docker run -v #{directory of host machine}:#{directory of conatiner}
```

```
D:\project\docker\sample001
λ docker run -it --rm -v %cd%/src:/host_mnt/d/project/docker/sample001/src --name python_contianer python_image /bin/bash
```

![alt tag](https://i.imgur.com/F6dXwfT.jpg)

## Make easier by docker-compose
docker-compose

In PowerShell, you use ${PWD},

docker-compose_pwrshell.yaml
```
version: "3"
services:
  app:
    container_name: "python_app"
    build:
      context: .
      dockerfile: ./Dockerfile
    image: python_ml:3.7.2
    volumes:
      - ${PWD}/src:/host_mnt/d/project/docker/sample001/src
      - ${PWD}/test:/host_mnt/d/project/docker/sample001/test
    ports:
      - 80:80
    tty: true
```

## What's docker-compose simply？？？？？

複数のコンテナを同時に立ち上げてくれるもの。
オプションを記述することで各コンテナの起動時の設定などができる。
今回は複数のコンテナではないですが、起動時のオプションをこちらに持たせておくことにしましょう。
docker-composeのあるディレクトリに移動して

以下のコマンドでイメージの作成からコンテナの起動まで全て行ってくれます。
```
D:\project\docker\sample001
λ  docker-compose --file docker-compose_pwrshell.yaml up -d
```
![alt tag](https://i.imgur.com/yxeqTcn.jpg)
![alt tag](https://i.imgur.com/yGGdqlv.jpg)

あとはコンテナの内部に以下のコマンドで入ります。
```
docker exec -it python_app /bin/sh -c "[ -e /bin/bash ] && /bin/bash || /bin/sh"
```

# Troubleshooting



Environment
==============================
``` 
o ASUS VivoBook S14, Core-i3 8th, RAM:4GB。
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [Dockerを使って機械学習の環境を作ろうとした話](https://qiita.com/oq-Yuki-op/items/3b7a0f1e27bbab56a95f)
* [Mount current directory as a volume in Docker on Windows 10](https://stackoverflow.com/questions/41485217/mount-current-directory-as-a-volume-in-docker-on-windows-10)

In Windows Command Line (cmd), you can mount the current directory like so:
```
docker run --rm -it -v %cd%:/usr/src/project gcc:4.9
```
In PowerShell, you use ${PWD}, which gives you the current directory:
```
docker run --rm -it -v ${PWD}:/usr/src/project gcc:4.9
```

* [Volume mounts in windows does not work](https://forums.docker.com/t/volume-mounts-in-windows-does-not-work/10693/161)

* [Errror mkdir /host_mnt/c: file exists when restarting docker container with mount](https://github.com/docker/for-win/issues/1560)
```
I solve it by simply run this safe command

docker volume prune
```

* [Running Windows 10 Ubuntu Bash in Cmder](https://gingter.org/2016/11/16/running-windows-10-ubuntu-bash-in-cmder/)
* [Windows Bash won't open in 10 Anniversary Update](https://superuser.com/questions/1112429/windows-bash-wont-open-in-10-anniversary-update)
>Prerequisites
Actually, you just need Bash on Ubuntu on Windows enabled and, of course Cmder. If you don’t have that, you can simply follow these instructions:
Install Bash On Ubuntu on Windows
Download and unzip Cmder to a convenient place on your disk. Then start Cmder.exe.

* [How to Install and Use the Linux Bash Shell on Windows 10](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/)

* [Installing Ubuntu on Windows 10 Without Using the Windows Store](https://www.dennisnguyen.net/2018/08/15/installing-ubuntu-on-windows-10-without-using-the-windows-store/)
* [Manually download Windows Subsystem for Linux distro packages](https://docs.microsoft.com/en-us/windows/wsl/install-manual)
```
Download using curl

curl.exe -L -o ubuntu-1604.appx https://aka.ms/wsl-ubuntu-1604
```

* [Windows Subsystem for Linux: Error 0x80070005 (Access Denied) October 11, 2017](https://adrift.io/2017/10/11/windows-subsystem-for-linux-error-0x80070005-access-denied/)
>That’s not cool though, we want to reach these things remotely and invoke things in an automation-friendly way. As it turns out, there are a couple of things preventing you from achieving this. Primarily:
Local launch and activation permissions on the Linux Subsystem / LXSS (as it’s referenced internally) DCOM configuration object.
Registry permissions which prevent you from modifying the above.
* [Can't install apps from Store, error 0x80070005 3/30/2018](https://answers.microsoft.com/en-us/windows/forum/windows_8-windows_store/cant-install-apps-from-store-error-0x80070005/1da61861-26ee-41b4-ada9-8ac516b0107a?page=3)

* []()
![alt tag]()