# Docker Compose
Multi-containers environment creation by Docker Compose.

# Introdcution
Web system consists of various serves such as WEB serve, AP serve and DB serve so that manages by Docker Compose.

## By docker-compose.yml definition, using command "docker-compose up" to start various containers.
![alt tag](https://camo.qiitausercontent.com/bb16ede939c3772533715a916b0be87588d18b2c/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3230393930392f37626433323263352d336632332d393762352d653936372d6161663831343237633962312e706e67)
* [【図解】Dockerの全体像を理解する -中編- 2019-01-22](https://qiita.com/kotaro-dr/items/88ec3a0e2d80d7cdf87a)

# Commands related Docker 
``` 
docker run --link 接続したいコンテナ名：エイリアス名 イメージ名 実行コマンド
docker-compose up
docker-compose scale コンテナ名=数
docker-compose ps
docker-compose logs
docker-compose run コンテナ名 実行コマンド
docker-compose start
docker-compose stop
docker-compose restart
docker-compose kill
docker-compose rm
``` 

# YAML format for docker-compose.yml

``` 
indent means hierarchy.
# means comment.
- means following definitions.
Container without image name in first line, assign container name.
(インデントなしの行の先頭に定義するコンテナの種類ごとにコンテナ名を指定する)
``` 

## image 

``` 
testserver:
 image: ubuntu
``` 
Container name is testserver.

## build

``` 
testserver:
 build: .
``` 
In docker-compose.yml, image／build must be assinged.

## command

> コマンド実行を行う場合はコマンドを指定する
``` 
testserver:
 image: ubuntu

 command: /bin/bash
``` 

## links／external_links container:alias

``` 
testserver:
 image: ubuntu

 links:
  - dbserver:mysql
``` 
links指定は同一のdocker-compose.ymlに定義があるコンテナのみ指定可能であることに対し、external_linksは外部の別のコンテナとリンク機能を使って連携するときに指定する
docker-compose.ymlを使用しない場合は、以下のコマンドでdocker run時にもリンク機能の指定が可能

## docker run --link 接続したいコンテナ名：エイリアス名 イメージ名 実行コマンド
引数で指定したコンテナに対し環境変数に自動的に設定してくれる
ただし、同一ホストマシン上で起動しているコンテナ間でしか、アクセスできないという制限がある

## ports／expose port number

「port num of host machine:port num of container」の形式でホストマシンのポートを明示的に指定することもできるが、記載がなければランダムなポートが使用される
``` 
testserver:
 image: ubuntu

 ports:
  - "80:80"
``` 

expose指定となる
その場合は、ホストマシンのポート番号は省略する
``` 
testserver:
 image: ubuntu

 expose:
  - "8000"
``` 

## volumes mount point

> ホスト側でマウントするパスを指定する場合は、「folder path of host machine:folder path of container」の形式で指定する
```
testserver:
 image: ubuntu

 volumes:
  - /var/lib/mysql
```

## volumes_from マウント先のコンテナ名

> volume of specific container をmountする場合に使用する
```
testserver:
 image: ubuntu

 volumes_from:
  - dataserver
```

## environment 環境変数名=値

> 環境変数を指定する場合に使用する
```
testserver:
 image: ubuntu

 environment:
  - HOGE=hoge
```

## container_name

```
testserver:
 image: ubuntu

 container_name: webserver
```

## labels 

> コンテナにラベルを付けるときに使用する
```
testserver:
 image: ubuntu

 labels:
  - "description=testserver"
```

# Command of docker-compose while container creations by docker-compose.yml


## docker-compose up
> docker-compose.ymlを使って複数のコンテナの生成／起動を行う

-f でdocker-compose.ymlのファイル指定が可能

-f がなければカレントにあるdocker-compose.ymlを使う

-d でバックグラウンド起動する
```
> docker-compose -f webdb-docker-compose.yml up -d
Creating wordpress_dbserver_1 ...
Creating wordpress_dbserver_1 ... done
Creating wordpress_webserver_1 ...
Creating wordpress_webserver_1 ... done
```

webdb-docker-compose.yml
```
webserver:
 image: wordpress
 ports:
  - "80:80"
 links:
  - dbserver:mysql

dbserver:
 image: mysql
 environment:
  MYSQL_ROOT_PASSWORD: password
```

Check process of conatiner
```
> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
d9c5c3b521a5        wordpress           "docker-entrypoint..."   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   wordpress_webserver_1
f02a94dcee33        mysql               "docker-entrypoint..."   About a minute ago   Up About a minute   3306/tcp             wordpress_dbserver_1
```
コンテナ名は正確には「実行フォルダ_docker-compose.ymlのコンテナ名_通番」なるらしい
そのためdocker-compose.yml記載のコンテナ名はサービス名と言われる

## docker-compose scale コンテナ名=数

> 起動コンテナ数を指定する時に使用する
```
> docker-compose scale webserver=1 dbserver=2
WARNING: The scale command is deprecated. Use the up command with the --scale flag instead.
Creating wordpress_webserver_1 ...
Creating wordpress_webserver_1 ... done
Creating wordpress_dbserver_1 ...
Creating wordpress_dbserver_2 ...
Creating wordpress_dbserver_1 ... done
Creating wordpress_dbserver_2 ... done
```

複数起動時に矛盾が発生する場合はエラーが発生する
例えばwebserverを2つ起動する場合は80ポートが競合する、、、  
```
> docker-compose scale webserver=2 dbserver=1
WARNING: The scale command is deprecated. Use the up command with the --scale flag instead.
WARNING: The "webserver" service specifies a port on the host. If multiple containers for this service are created on a single host, the port will clash.
Creating wordpress_webserver_1 ...
Creating wordpress_webserver_2 ...
Creating wordpress_webserver_1 ... error
Creating wordpress_webserver_2 ... done

ERROR: for wordpress_webserver_1  Cannot start service webserver: driver failed programming external connectivity on endpoint wordpress_webserver_1 (e1a6f29e4ec9f31c40b5470cf9109c98765c5a7e03a10013758063beb54b8436): Bind for 0.0.0.0:80 failed: port is already allocated
ERROR: Cannot start service webserver: driver failed programming external connectivity on endpoint wordpress_webserver_1 (e1a6f29e4ec9f31c40b5470cf9109c98765c5a7e03a10013758063beb54b8436): Bind for 0.0.0.0:80 failed: port is already allocated
```

## docker-compose ps

複数コンテナの一覧表示を行う
docker psコマンドと比較すると気持ちシンプル表示になっている？

```
> docker-compose ps
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
wordpress_dbserver_1    docker-entrypoint.sh mysqld      Up      3306/tcp
wordpress_dbserver_2    docker-entrypoint.sh mysqld      Up      3306/tcp
wordpress_webserver_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp

> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
d9f2dec9af02        mysql               "docker-entrypoint..."   5 minutes ago       Up 5 minutes        3306/tcp             wordpress_dbserver_1
cde58d78dc3d        mysql               "docker-entrypoint..."   5 minutes ago       Up 5 minutes        3306/tcp             wordpress_dbserver_2
816fcbe2c4f7        wordpress           "docker-entrypoint..."   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp   wordpress_webserver_1
```

## docker-compose logs

コンテナのログを確認する場合に使用する
コンテナごとのログが表示される
```
> docker-compose logs
Attaching to wordpress_dbserver_1, wordpress_dbserver_2, wordpress_webserver_1
webserver_1  | WordPress not found in /var/www/html - copying now...
webserver_1  | Complete! WordPress has been successfully copied to /var/www/html
dbserver_1   | Initializing database
webserver_1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
dbserver_2   | Initializing database
dbserver_1   | 2018-01-27T02:09:56.866053Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
webserver_1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
dbserver_2   | 2018-01-27T02:09:56.887342Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
～～～省略～～～
```

## docker-compose run コンテナ名 実行コマンド

起動したコンテナに新たにコマンドを実行する場合に使用する
runのあとにコンテナ名を指定すると指定したコンテナだけの操作となる（他のコマンドでも同様）
コンテナ内でコマンド実行したい場合など/bin/bashを実行する
```
> docker-compose run webserver /bin/bash
Starting wordpress_dbserver_1 ... done
Starting wordpress_dbserver_2 ... done
root@8271d38f7036:/var/www/html#
```

## docker-compose start

複数のコンテナを一括で起動する場合に使用する
```
> docker-compose start
Starting dbserver  ... done
Starting webserver ... done

> docker-compose ps
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
wordpress_dbserver_1    docker-entrypoint.sh mysqld      Up      3306/tcp
wordpress_dbserver_2    docker-entrypoint.sh mysqld      Up      3306/tcp
wordpress_webserver_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

## docker-compose stop

複数のコンテナを一括で停止する場合に使用する
```
> docker-compose stop
Stopping wordpress_dbserver_1  ... done
Stopping wordpress_dbserver_2  ... done
Stopping wordpress_webserver_1 ... done
```

## docker-compose restart

複数のコンテナを一括で再起動する場合に使用する
```
> docker-compose restart
Restarting wordpress_dbserver_1  ... done
Restarting wordpress_dbserver_2  ... done
Restarting wordpress_webserver_1 ... done
```

## docker-compose kill

複数のコンテナを一括で強制停止する場合に使用する
```
> docker-compose kill
Killing wordpress_dbserver_1  ... done
Killing wordpress_dbserver_2  ... done
Killing wordpress_webserver_1 ... done

> docker-compose ps
        Name                       Command                State     Ports
-------------------------------------------------------------------------
wordpress_dbserver_1    docker-entrypoint.sh mysqld      Exit 137
wordpress_dbserver_2    docker-entrypoint.sh mysqld      Exit 137
wordpress_webserver_1   docker-entrypoint.sh apach ...   Exit 137
```

## docker-compose rm

複数のコンテナを一括で削除する場合に使用する
```
> docker-compose rm
Going to remove wordpress_dbserver_1, wordpress_dbserver_2, wordpress_webserver_1
Are you sure? [yN] y
Removing wordpress_dbserver_1      ... done
Removing wordpress_dbserver_2      ... done
Removing wordpress_webserver_1     ... done
```

Environment
==============================
``` 
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [WindowsでDocker Composeを使ってマルチコンテナ環境を作ってみる](https://qiita.com/fkooo/items/69a050da265c712763ec)
* [【図解】Dockerの全体像を理解する -中編- 2019-01-22](https://qiita.com/kotaro-dr/items/88ec3a0e2d80d7cdf87a)

* []()
![alt tag]()