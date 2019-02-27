# Crack Docker


Goal_01
==============================
Four elements as below:

⓪Docker Engine
①Image
②Container
③Docker Registry (DockerHub)

![alt tag](https://camo.qiitausercontent.com/11f8c7610bb55b39d3c15869154de12e27b4294f/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3230393930392f34353866333161662d646232302d666134612d316537622d3539666161666630376138302e706e67)

Goal_02
==============================
![alt tag](https://camo.qiitausercontent.com/2d086e006913c2d1088fd3dae1460ef8e792f884/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f3230393930392f61653739303839352d363836302d623466632d363165642d3638303561306662653966352e706e67)

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
* [【図解】Dockerの全体像を理解する -前編-](https://qiita.com/kotaro-dr/items/b1024c7d200a75b992fc)
* [【図解】Dockerの全体像を理解する -中編-](https://qiita.com/kotaro-dr/items/88ec3a0e2d80d7cdf87a)
* [【図解】Dockerの全体像を理解する -後編-](https://qiita.com/kotaro-dr/items/40106f13d47bfcbc2572)

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