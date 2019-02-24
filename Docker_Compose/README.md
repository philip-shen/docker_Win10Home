# Docker Compose
Multi-containers environment creation by Docker Compose.

Introdcution
==============================
Web system consists of various serves such as WEB serve, AP serve and DB serve so that manages by Docker Compose.

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

* YAML format for docker-compose.yml

``` 
indent means hierarchy.
# means comment.
- means following definitions.
Container without image name in first line, assign container name.
(インデントなしの行の先頭に定義するコンテナの種類ごとにコンテナ名を指定する)
``` 

* image 

``` 
testserver:
 image: ubuntu
``` 
Container name is testserver.

* build

``` 
testserver:
 build: .
``` 
In docker-compose.yml, image／build must be assinged.

* command

コマンド実行を行う場合はコマンドを指定する
``` 
testserver:
 image: ubuntu

 command: /bin/bash
``` 

* links／external_links container:alias

``` 
testserver:
 image: ubuntu

 links:
  - dbserver:mysql
``` 
links指定は同一のdocker-compose.ymlに定義があるコンテナのみ指定可能であることに対し、external_linksは外部の別のコンテナとリンク機能を使って連携するときに指定する
docker-compose.ymlを使用しない場合は、以下のコマンドでdocker run時にもリンク機能の指定が可能

* docker run --link 接続したいコンテナ名：エイリアス名 イメージ名 実行コマンド
引数で指定したコンテナに対し環境変数に自動的に設定してくれる
ただし、同一ホストマシン上で起動しているコンテナ間でしか、アクセスできないという制限がある

* ports／expose port number

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

* volumes mount point

ホスト側でマウントするパスを指定する場合は、「folder path of host machine:folder path of container」の形式で指定する
```
testserver:
 image: ubuntu

 volumes:
  - /var/lib/mysql
```

* volumes_from マウント先のコンテナ名

volume of specific container をmountする場合に使用する
```
testserver:
 image: ubuntu

 volumes_from:
  - dataserver
```

* environment 環境変数名=値

環境変数を指定する場合に使用する
```
testserver:
 image: ubuntu

 environment:
  - HOGE=hoge
```

* container_name

```
testserver:
 image: ubuntu

 container_name: webserver
```

* labels 

コンテナにラベルを付けるときに使用する
```
testserver:
 image: ubuntu

 labels:
  - "description=testserver"
```

* Command of docker-compose while container creations by docker-compose.yml


*docker-compose up*
docker-compose.ymlを使って複数のコンテナの生成／起動を行う

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

コンテナ起動を確認
    ```
    > docker ps
    CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
    d9c5c3b521a5        wordpress           "docker-entrypoint..."   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   wordpress_webserver_1
    f02a94dcee33        mysql               "docker-entrypoint..."   About a minute ago   Up About a minute   3306/tcp             wordpress_dbserver_1
    ```
コンテナ名は正確には「実行フォルダ_docker-compose.ymlのコンテナ名_通番」なるらしい
そのためdocker-compose.yml記載のコンテナ名はサービス名と言われる

*docker-compose scale コンテナ名=数*

起動コンテナ数を指定する時に使用する
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
* [WindowsでDocker Composeを使ってマルチコンテナ環境を作ってみる](https://qiita.com/fkooo/items/69a050da265c712763ec)


* []()
![alt tag]()