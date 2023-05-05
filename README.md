
Table of Contents
=================
   * [Table of Contents](#table-of-contents)
   * [Docker_Win10Home](#docker_win10home)
   * [Docker related images of foundation function](#docker-related-images-of-foundation-function)
   * [Implementation](#implementation)
      * [Step 1](#step-1)
      * [Step 2](#step-2)
      * [Step 3](#step-3)
      * [Step 4](#step-4)
      * [Step 5](#step-5)
      * [Step 6](#step-6)
      * [Step 7](#step-7)
      * [Windows Subsystem for Linux Installation Guide for Windows 10](#windows-subsystem-for-linux-installation-guide-for-windows-10)
         * [Simplified install](#simplified-install)
         * [Manual install](#manual-install)
            * [Step 1 - Enable the Windows Subsystem for Linux](#step-1---enable-the-windows-subsystem-for-linux)
            * [Step 2 - Check requirements for running WSL 2](#step-2---check-requirements-for-running-wsl-2)
            * [Step 3 - Enable Virtual Machine feature](#step-3---enable-virtual-machine-feature)
            * [Step 4 - Download the Linux kernel update package](#step-4---download-the-linux-kernel-update-package)
            * [Step 5 - Set WSL 2 as your default version](#step-5---set-wsl-2-as-your-default-version)
            * [Step 6 - Install your Linux distribution of choice  ```](#step-6---install-your-linux-distribution-of-choice--)
            * [Install Windows Terminal (optional)](#install-windows-terminal-optional)
            * [Set your distribution version to WSL 1 or WSL 2](#set-your-distribution-version-to-wsl-1-or-wsl-2)
            * [Troubleshooting installation](#troubleshooting-installation)
               * [Installation failed with error 0x80070003](#installation-failed-with-error-0x80070003)
               * [WslRegisterDistribution failed with error 0x8007019e](#wslregisterdistribution-failed-with-error-0x8007019e)
               * [Installation failed with error 0x80070003 or error 0x80370102](#installation-failed-with-error-0x80070003-or-error-0x80370102)
               * [Error when trying to upgrade: Invalid command line option: wsl --set-version Ubuntu 2](#error-when-trying-to-upgrade-invalid-command-line-option-wsl---set-version-ubuntu-2)
               * [The requested operation could not be completed due to a virtual disk system limitation. Virtual hard disk files must be uncompressed and unencrypted and must not be sparse.](#the-requested-operation-could-not-be-completed-due-to-a-virtual-disk-system-limitation-virtual-hard-disk-files-must-be-uncompressed-and-unencrypted-and-must-not-be-sparse)
               * [The term 'wsl' is not recognized as the name of a cmdlet, function, script file, or operable program.](#the-term-wsl-is-not-recognized-as-the-name-of-a-cmdlet-function-script-file-or-operable-program)
               * [Error: Windows Subsystem for Linux has no installed distributions.](#error-windows-subsystem-for-linux-has-no-installed-distributions)
               * [Error: This update only applies to machines with the Windows Subsystem for Linux.](#error-this-update-only-applies-to-machines-with-the-windows-subsystem-for-linux)
               * [Error: WSL 2 requires an update to its kernel component. For information please visit <a href="https://aka.ms/wsl2kernel" rel="nofollow">https://aka.ms/wsl2kernel</a> .](#error-wsl-2-requires-an-update-to-its-kernel-component-for-information-please-visit-httpsakamswsl2kernel-)
      * [Update-WSL2](#update-wsl2)
   * [TroubleShooting](#troubleshooting)
   * [Environment](#environment)
   * [Reference](#reference)
   * [Table of Contents](#table-of-contents-1)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

# Docker_Win10Home
Docker tutorial that is installed on Win10 Home by Hyper-V rather than toolbox

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

## Step 6 Docker Installation  
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

## Windows Subsystem for Linux Installation Guide for Windows 10  
[Windows Subsystem for Linux Installation Guide for Windows 10 06/09/2021](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

### Simplified install 
``` 
The wsl --install simplified install command requires that you join the Windows Insiders Program and install a preview build of Windows 10 (OS build 20262 or higher), but eliminates the need to follow the manual install steps. All you need to do is open a command window with administrator privileges and run wsl --install, after a restart you will be ready to use WSL.
``` 

### Manual install  

#### Step 1 - Enable the Windows Subsystem for Linux  
``` 
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
``` 

#### Step 2 - Check requirements for running WSL 2  
```
    For x64 systems: Version 1903 or higher, with Build 18362 or higher.
    For ARM64 systems: Version 2004 or higher, with Build 19041 or higher.
    Builds lower than 18362 do not support WSL 2. Use the Windows Update Assistant to update your version of Windows.
``` 
``` 
To check your version and build number, select Windows logo key + R, type winver, select OK. Update to the latest Windows version in the Settings menu.
``` 

#### Step 3 - Enable Virtual Machine feature  
``` 
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
``` 

#### Step 4 - Download the Linux kernel update package  
[WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

#### Step 5 - Set WSL 2 as your default version  
``` 
wsl --set-default-version 2
``` 

#### Step 6 - Install your Linux distribution of choice  ``` 

``` 
Open the Microsoft Store and select your favorite Linux distribution.
``` 
<img src="https://docs.microsoft.com/en-us/windows/wsl/media/store.png" width="700" height="500">  

``` 
You will then need to create a user account and password for your new Linux distribution.
``` 
<img src="https://docs.microsoft.com/en-us/windows/wsl/media/ubuntuinstall.png" width="900" height="200">  

#### Install Windows Terminal (optional)
<img src="https://docs.microsoft.com/en-us/windows/wsl/media/terminal.png" width="800" height="500">  


#### Set your distribution version to WSL 1 or WSL 2  
``` 
wsl --list --verbose
``` 

``` 
wsl --set-version <distribution name> <versionNumber>
``` 

``` 
Additionally, if you want to make WSL 2 your default architecture you can do so with this command:
``` 

``` 
wsl --set-default-version 2
``` 

#### Troubleshooting installation  
[Troubleshooting installation](https://docs.microsoft.com/en-us/windows/wsl/install-win10#troubleshooting-installation)

##### Installation failed with error 0x80070003  
##### WslRegisterDistribution failed with error 0x8007019e  
##### Installation failed with error 0x80070003 or error 0x80370102   
##### Error when trying to upgrade: Invalid command line option: wsl --set-version Ubuntu 2  
##### The requested operation could not be completed due to a virtual disk system limitation. Virtual hard disk files must be uncompressed and unencrypted and must not be sparse.  
##### The term 'wsl' is not recognized as the name of a cmdlet, function, script file, or operable program.  
##### Error: Windows Subsystem for Linux has no installed distributions.  
##### Error: This update only applies to machines with the Windows Subsystem for Linux.  
##### Error: WSL 2 requires an update to its kernel component. For information please visit https://aka.ms/wsl2kernel . 


## Update-WSL2   
[Installing Docker on Windows 10 Home](https://forums.docker.com/t/installing-docker-on-windows-10-home/11722/64)  
[Install Docker On Windows 10 Home](https://gist.github.com/talon/4191def376c9fecae78815454bfe661c)
``` 
    Hi!

    We have now released a version of Docker Desktop Edge that allows users of Windows Insider Preview (Win Home 19018 or higher) to use Docker Desktop with WSL 2. If you are trying this out and have issues feel free to drop issues on the for-win Github Repo!

    Thanks Ben
``` 
– https://forums.docker.com/t/installing-docker-on-windows-10-home/11722/70


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
* [如何在 Windows 10 Home 安裝 Docker ? by ToolBox](https://old-oomusou.goodjack.tw/docker/toolbox/)
* [Dockerの基本機能と全体像のイメージを整理してみる](https://qiita.com/fkooo/items/934c7b6f1f0c0e8d1b21)
* [Dockerを使って機械学習の環境を作ろうとした話](https://qiita.com/oq-Yuki-op/items/3b7a0f1e27bbab56a95f)

* [在 Win10 安裝 Jekyll 部落格網站產生器及簡單起步教學](https://blog.jaycetyle.com/2018/01/jekyll-on-win10/)

* []()
![alt tag]()

<img src="" width="400" height="500">  



