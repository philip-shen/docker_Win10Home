# Docker_Win10Home
Docker tutorial that is installed on Win10 Home by Hyper-V rather than toolbox

# Table of Contents  
[Docker related images of foundation function](#docker-related-images-of-foundation-function)  

[Implementation](#implementation)  
[Step 1](#step-1)  
[Step 2](#step-2)  
[Step 3](#step-3)  
[Step 4](#step-4)  
[Step 5](#step-5)  
[Step 6](#step-6)  
[Step 7](#step-7)  

[TroubleShooting](#troubleshooting)  
[Environment](#environment)  
[Reference](#reference)  

Docker related images of foundation function
==============================
![alt tag](https://camo.qiitausercontent.com/e29da7153d29910bfc4159c18e943269630ced88/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3138383534342f38336266646465372d353462332d323330632d653236352d3435343162333461393965312e706e67)
* [Dockerの基本機能と全体像のイメージを整理してみる](https://qiita.com/fkooo/items/934c7b6f1f0c0e8d1b21)

# Implementation

## Step 1
Make sure CPU virtualization fucntion of BIOS on your Laptop is enable.
* [Windows 10 - How to enter BIOS configuration?](https://www.youtube.com/watch?v=HQXFd0CN4s8&feature=youtu.be)

And refer Task Manager:
![alt tag](https://i.imgur.com/LEtsMSW.jpg)

## Step 2
Download hyper-v.bat from the project.
And exec the bat file under administration privelge.
![alt tag](https://i.imgur.com/sq9RzXR.gif)

## Step 3
Modify registry key value by regedit
``` 
Change in \HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion :
EditionID: Core --> Professional
ProductName: Windows 10 Home --> Windows 10 Pro
``` 
![alt tag](https://i.imgur.com/Y88YvjW.jpg)

## Step 4
Make sure that Hyper-V is enable by Control Panel on Win 10
![alt tag](https://i.imgur.com/6BOFoDH.jpg)

## Step 5 
Excute containers.bat on console by Administrator privelge.
![alt tag](https://i.imgur.com/OWWGrUZ.jpg)

> 3/4/2019 Prevent addtional exception happened to add.

## Step 6
Download the lastest version of Windows-installer from Docker website:
* [https://www.docker.com/docker-windows](https://www.docker.com/products/docker-desktop)

And install "Docker for Windows Installer.exe"
![alt tag](https://i.imgur.com/MQZ2kFV.jpg)

## Step 7
Make sure Docker successful installation.

![alt tag](https://i.imgur.com/ac3U1m0.jpg)

``` 
docker --version
docker-compose --version
``` 
![alt tag](https://i.imgur.com/v50wg4I.jpg)

Setup "Shared Drives"
![alt tag](https://i.imgur.com/nkCitB2.jpg)

Enjoy Docker!

TroubleShooting
==============================
* Client.Timeout exceeded while awaiting headers
![alt tag](https://i.stack.imgur.com/4MSYK.jpg)

Unfortunately answers above didn't help in my case, but restarting Docker did.

![alt tag](https://i.stack.imgur.com/WORpt.png)

* [Docker Toolbox Tutorial Client.Timeout exceeded while awaiting headers](https://stackoverflow.com/questions/46822391/docker-toolbox-tutorial-client-timeout-exceeded-while-awaiting-headers)

Environment
==============================
``` 
o ASUS VivoBook S14, Core-i3 8th, RAM:4GB。
o Win 10 Home Version1803(OS Build 17134.590) 。
o Docker Desktop 2.0.0.3
``` 

Reference 
==============================
* [Docker 基本教學](https://github.com/twtrubiks/docker-tutorial)
* [Installing Docker on Windows 10 Home](https://forums.docker.com/t/installing-docker-on-windows-10-home/11722/24)
* [如何在 Windows 10 Home 安裝 Docker ? by ToolBox](https://oomusou.io/docker/toolbox/)
* [Dockerの基本機能と全体像のイメージを整理してみる](https://qiita.com/fkooo/items/934c7b6f1f0c0e8d1b21)
* [Dockerを使って機械学習の環境を作ろうとした話](https://qiita.com/oq-Yuki-op/items/3b7a0f1e27bbab56a95f)

* [在 Win10 安裝 Jekyll 部落格網站產生器及簡單起步教學](https://blog.jaycetyle.com/2018/01/jekyll-on-win10/)

* []()
![alt tag]()