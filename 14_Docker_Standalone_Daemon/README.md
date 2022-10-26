Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Purpose](#purpose)
   * [Docker daemon and Docker CLI](#docker-daemon-and-docker-cli)
      * [Windows Containers](#windows-containers)
   * [Running Docker in WSL Preview](#running-docker-in-wsl-preview)
      * [Step](#step)
   * [Troubleshooting](#troubleshooting)
   * [Reference](#reference)
   * [h1 size](#h1-size)
      * [h2 size](#h2-size)
         * [h3 size](#h3-size)
            * [h4 size](#h4-size)
               * [h5 size](#h5-size)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

# Purpose  
Take note of Docker Daemon on Windows 10


# Docker daemon and Docker CLI
[不安裝 Docker Desktop 使用 Windows Docker 容器 2021/10/15](https://kheresy.wordpress.com/2021/10/15/run-windows-container-without-docker-desktop/)

```
curl.exe -o docker.zip -LO https://download.docker.com/win/static/stable/x86_64/docker-20.10.13.zip 
Expand-Archive docker.zip -DestinationPath C:\
 

[Environment]::SetEnvironmentVariable("Path", "$($env:path);C:\docker", [System.EnvironmentVariableTarget]::Machine)
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
 
New-LocalGroup -Name docker-users 
dockerd --register-service -G docker-users --config-file C:\docker\daemon.json  
Start-Service docker

```

* 1 透過 curl 下載 docker 的檔案
* 2 透過「Expand-Archive」來解壓縮，這邊會解壓縮成「C:\docker」
* 3 透過「[Environment]::SetEnvironmentVariable()」設定系統環境變數
* 4 更新目前階段的環境變數 $env:Path
* 5 將 dockerd 註冊成 Windows service，名字會是「Docker Engine」
* 6 啟動 docker service
* 7 測試

```
理論上，透過有系統管理者權限的 PowerShell 執行上面的指令，就可以完成 Docker 環境的建立了。

不過，實際上在第五步、註冊 Windows Service 的時候，還要再加一些參數，才會比較實用。
```

## Windows Containers
[Running Windows and Linux containers without Docker Desktop September 4, 2021](https://lippertmarkus.com/2021/09/04/containers-without-docker-desktop/)

```
# Optionally enable required Windows features if needed
Enable-WindowsOptionalFeature -Online -FeatureName containers –All
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V –All

curl.exe -o docker.zip -LO https://download.docker.com/win/static/stable/x86_64/docker-20.10.13.zip 
Expand-Archive docker.zip -DestinationPath C:\
[Environment]::SetEnvironmentVariable("Path", "$($env:path);C:\docker", [System.EnvironmentVariableTarget]::Machine)
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
dockerd --register-service
Start-Service docker
docker run hello-world
```


# Running Docker in WSL Preview
[Run Docker in WSL (Windows 10/11) without Docker Desktop Oct 29, 2021](https://medium.com/geekculture/run-docker-in-windows-10-11-wsl-without-docker-desktop-a2a7eb90556d)

```
wsl --version
```
<img src="https://miro.medium.com/max/720/1*Dg8gq0c-WS-Go5MISbd_dA.png" width="500" height="200">

```
sudo nano /etc/wsl.conf
```

and 
```
[boot]
systemd=true
```

## Step
* 1. Update the local repository.
```
sudo apt update
```

* 2. Install Docker.
```
sudo apt install docker.io -y
```

* 3. Check Docker installation.
```
docker --version
```

* 4. 
```
sudo usermod -aG docker $USER
```

```
wsl --shutdown
```

* 5. Validate Docker installation
```
docker run hello-world
```


# Troubleshooting


# Reference


* []()
![alt tag]()
<img src="" width="400" height="500">

# h1 size

## h2 size

### h3 size

#### h4 size

##### h5 size

*strong*strong  
**strong**strong  

> quote  
> quote

- [ ] checklist1
- [x] checklist2

* 1
* 2
* 3

- 1
- 2
- 3

