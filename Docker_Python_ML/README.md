# Machine Learning in Python under Docker


Goal
==============================

Hand on Docker operation, 
Practice Docker to build a image to creat a conatiner


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

``` d:\project\docker\sample001>docker build -t python_image .
Sending build context to Docker daemon   5.12kB
Step 1/7 : FROM python:3.7.2-slim
3.7.2-slim: Pulling from library/python
6ae821421a7d: Already exists
759f8891d8a3: Pull complete
4e25683ec213: Pull complete
e545f39a0e31: Pull complete
43d66734da83: Pull complete
Digest: sha256:64d2fc0ac09ae6ec501651de34c5921f0dbfcfbe28b00ab34affeccf4f547464
Status: Downloaded newer image for python:3.7.2-slim
 ---> bc5085cee55b
Step 2/7 : ARG root_directory=/host_mnt/d/project/docker/sample001
 ---> Running in b85a1023342d
Removing intermediate container b85a1023342d
 ---> 583eb97e95c3
Step 3/7 : RUN apt-get update && apt-get install -y     python3     python3-pip     && apt-get clean     && rm -rf /var/lib/apt/lists/*     && pip install --upgrade pip

~~~省略~~

Step 4/7 : COPY . ${root_directory}/
 ---> 3d30e25a6142
Step 5/7 : WORKDIR ${root_directory}/pip/
 ---> Running in 30e6f4fb6cfb
Removing intermediate container 30e6f4fb6cfb
 ---> 2946924d0449
Step 6/7 : RUN pip install -r requirements.txt
 ---> Running in f48b3cec4be4

~~~省略~~

Step 7/7 : WORKDIR ${root_directory}
 ---> Running in 7d9c943bed82
Removing intermediate container 7d9c943bed82
 ---> b0a329f1a53d
Successfully built b0a329f1a53d
Successfully tagged python_image:latest
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
``` 

### Check image if exists or not?
``` 
d:\project\docker\sample001>docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
python_image               latest              b0a329f1a53d        9 minutes ago       826MB
python                     3.7.2-slim          bc5085cee55b        3 days ago          143MB
``` 

``` 
d:\project\docker\sample001>docker run -it --rm --name python_container python_image  /bin/bash
root@c9cabcd8e2ca:/D:\project\docker\sample001/pip/D:\project\docker\sample001# pwd
/D:\project\docker\sample001/pip/D:\project\docker\sample001
root@c9cabcd8e2ca:/D:\project\docker\sample001/pip/D:\project\docker\sample001# cd ..
root@c9cabcd8e2ca:/D:\project\docker\sample001/pip# ls
D:\project\docker\sample001  requirements.txt
root@c9cabcd8e2ca:/D:\project\docker\sample001/pip# cd ..
root@c9cabcd8e2ca:/D:\project\docker\sample001# history
    1  pwd
    2  ls
    3  ls -al
    4  cd ..
    5  ls
    6  cd ..
    7  history
root@c9cabcd8e2ca:/D:\project\docker\sample001# !2
ls
Dockerfile  pip  src  test
``` 

## Excute hello.py
``` 
root@c9cabcd8e2ca:/D:\project\docker\sample001#python src/hello.py
hello
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
buildをする際にイメージの中にファイルを含めています。

つまり、このイメージをコンテナ化してもイメージに含まれているファイルしかコンテナには存在しないのです。

```
$ docker run -v #{directory of host machine}:#{directory of conatiner}
```

```

```

## Make easier by docker-compose
docker-compose

In Windows Command Line (cmd),
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
      - %cd%/src:/host_mnt/d/project/docker/sample001/src
      - %cd%/test:/host_mnt/d/project/docker/sample001/test
    ports:
      - 80:80
    tty: true
```

In PowerShell, you use ${PWD},
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
      - ${PWD}/src:/sample001/src
      - ${PWD}/test:/sample001/test
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
$ docker-compose up -dWARNING: The PWD variable is not set. Defaulting to a blank string.
Creating network "sample001_default" with the default driver
Building app
Step 1/6 : FROM python:3.7.2-slim
 ---> bc5085cee55b
Step 2/6 : ARG root_directory=/host_mnt/d/project/docker/sample001
 ---> Using cache
 ---> 02e0712af72b
Step 3/6 : RUN apt-get update && apt-get install -y     python3     python3-pip     && apt-get clean     && rm -rf /var/lib/apt/lists/*     && pip install --upgrade pip
 ---> Using cache
 ---> 98c0553742f9
Step 4/6 : COPY . ${root_directory}/
 ---> 23a11feb628d
Step 5/6 : WORKDIR ${root_directory}/pip/
 ---> Running in 20ee6b399e75
Removing intermediate container 20ee6b399e75
 ---> daf6ce3832a0
Step 6/6 : RUN pip install -r requirements.txt

~~~省略~~

Successfully installed atomicwrites-1.3.0 attrs-18.2.0 cycler-0.10.0 kiwisolver-1.0.1 matplotlib-3.0.2 more-itertools-6.0.0 numpy-1.16.2 pandas-0.24.1 pluggy-0.9.0 py-1.8.0 pyparsing-2.3.1 pytest-4.3.0 python-dateutil-2.8.0 pytz-2018.9 six-1.12.0
Removing intermediate container 058f402b26a9
 ---> 498d46b608d3
Successfully built 498d46b608d3
Successfully tagged python_ml:3.7.2
WARNING: Image for service app was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating python_app ... done
```

あとはコンテナの内部に以下のコマンドで入ります。
```
docker exec -it python_app /bin/sh -c "[ -e /bin/bash ] && /bin/bash || /bin/sh"
```

```
root@fc39ff57ea37:/# cat /host_mnt/d/project/docker/sample001/src/hello.py
cat: /host_mnt/d/project/docker/sample001/src/hello.py: No such file or directory
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