# Stop Memory Leak of Docker @ Win10

Goal
==============================
Solve to Memory Leakage of Docker @ Win10


Table of Contents
==============================
[一、限制 NonPagedPoolSize 大小。]()  
[二、停用 Windows Network Data 使用 Monitor Driver](#%E4%BA%8C%E5%81%9C%E7%94%A8-windows-network-data-%E4%BD%BF%E7%94%A8-monitor-driver)  
[三、停用 Null Service](#%E4%B8%89%E5%81%9C%E7%94%A8-null-service)  

Procedures
==============================
[追追追:快爆炸99%伺服器記憶體用到哪裡去了?  8/29/2018](https://blog.kkbruce.net/2018/08/why-memory-usage-99-percentage.html#more)  
## 一、限制 NonPagedPoolSize 大小。  
[NonPagedPoolSize 09/10/2008](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc976155(v%3dtechnet.10)) 
### 取得 NonPagedPoolSize 預設值： 
```
Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" -Name NonPagedPoolSize

NonPagedPoolSize : 0
```
### 設定 NonPagedPoolSize 值：  
```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" -Name "NonPagedPoolSize" -Value 1024
```

## 二、停用 Windows Network Data 使用 Monitor Driver  
### Ndu 的 Start 預設值應該為 2。
```
Get-ItemProperty -Path "HKLM:\SYSTEM\ControlSet001\Services\Ndu\" -Name Start

Start        : 2
```
### 修改為 4。
```
Set-ItemProperty -Path "HKLM:\SYSTEM\ControlSet001\Services\Ndu\" -Name Start -Value 4
```
*Windows 10 有此機碼。但我在 Server Core 上找不到此機碼。*  

## 三、停用 Null Service  
### Null 取得 Start 預設值應該為 1。  
```
Get-ItemProperty -Path "HKLM:\SYSTEM\ControlSet001\Services\Null" -Name Start

Start        : 1
```
### 修改為 4，以停用 Null Session Share。  
```
Set-ItemProperty -Path "HKLM:\SYSTEM\ControlSet001\Services\Null" -Name Start -Value 4
```
以我們的情境，第一個是比較合適的設定值。以下是測試一天後 Non-paged pool，由原本的近 12 GB 的使用量，現在限制在一定的使用量之內。

Troubleshooting
==============================
## Disable Docker  
![alt tag](https://i.imgur.com/5xU2lMH.jpg)    
## Enable docker  (Nonpage:288M (utility rate:58%) --> Nonpage:296M(utility rate:84%))
![alt tag](https://i.imgur.com/BosipoZ.jpg)  
## Modify NonPagedPoolSize Value
![alt tag](https://i.imgur.com/FDyCoVp.jpg)  


Reference 
==============================

* [Using PoolMon to Find a Kernel-Mode Memory Leak 05/23/2017](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/using-poolmon-to-find-a-kernel-mode-memory-leak)  
* [PoolMon Examples 2017/07/02](https://docs.microsoft.com/zh-tw/windows-hardware/drivers/devtest/poolmon-examples)  
* [Memory Pools 2018/05/31](https://docs.microsoft.com/zh-tw/windows/desktop/Memory/memory-pools)  
* [Pushing the Limits of Windows: Paged and Nonpaged Pool March 10, 2009](https://blogs.technet.microsoft.com/markrussinovich/2009/03/10/pushing-the-limits-of-windows-paged-and-nonpaged-pool/)  
```
又在 TechNet Mark's Blog 有比較詳細的 Memory 說明，其中兩段大至可以了解 nonpaged pool 的作用：

    The kernel and device drivers use nonpaged pool to store data that might be accessed when the system can’t handle page faults.
    Nonpaged pool is therefore always kept present in physical memory and nonpaged pool virtual memory is assigned physical memory. Common system data structures stored in nonpaged pool include the kernel and objects that represent processes and threads, synchronization objects like mutexes, semaphores and events, references to files, which are represented as file objects, and I/O request packets (IRPs), which represent I/O operations.

回頭看看開頭第一句，這些 Disk I/O 與 Network I/O 確實是由大量的非同步物件在操作，而且這些操作都是以1秒為單位。這樣解釋與我們操作行為就非常合理了。那麼接下來就是如何解決的問題了。
```

* [Download Debugging Tools for Windows 2019/01/24](https://docs.microsoft.com/zh-tw/windows-hardware/drivers/debugger/debugger-download-tools)  
* []()  


* []()  
![alt tag]()  

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