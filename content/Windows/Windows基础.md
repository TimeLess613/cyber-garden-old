---
tags:
  - IT/Windows
---
## 文件系统

C盘
- PerfLogs：存储系统问题和其他相关性能的报告
- Windows：%systemroot%。操作系统的代码和一些实用工具

## 文件权限

- [icacls命令](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/icacls)：Integrity Control Access Control Lists（完整性控制ACLs）
	- 即文件右键属性-安全的内容
- BUILTIN\Users：表示所有本地用户。
- 继承：即继承上一个文件夹的权限
	- 不止看文件权限，还可以看文件夹的权限：`icacls c:\MyPrograms`【可理解为ls -l】
![[Pasted image 20240309153403.png]]


## SID&RID

S-1-5-domain-RID

![[Pasted image 20240309153549.png]]

## 认证

> 认证是使用[[LSA]]（Local Security Authority）完成的。LSA 是一个受保护的子系统，用于跟踪计算机系统上的安全策略和帐户。它还维护有关计算机上本地安全各个方面的信息。

[[AD域认证协议]]



## 凭据

**即一组账号密码。**

![[cmd#cmdkey]]




## 服务器类型与角色

![[Pasted image 20240309154020.png]]




## 工作组

- 按功能分类的逻辑集合，对等式网络，各计算机还是各自管理自己的资源。
- 用于小规模网络。

## SAM

安全账号管理器（Security Account Manager）
位于：`%SystemRoot%\system32\config\sam`

本地认证（相对于Net-NTLM网络认证协议）。

- SAM文件即账号密码数据库文件。包含所有组、账户的信息，包括密码的NTLM hash、账户的SID等
- 登陆Windows系统后SAM是被锁死的，不能复制、移动，打开是乱码或空白
- **当我们登录系统的时候，系统会自动地和SAM对比NTLM hash**

如果你要在公司中的另一台计算机上登陆你的账号，那你就得在另一台计算机上的SAM数据库中加入你的账户信息，如果很多台……所以我们就要定义一个有安全边界的计算机集合，这个集合就是域。【即把设置、认证等资源抽象为中介】



## PE

Portable Executable，即可执行文件，如exe等。

> 因为 Windows NT 出名的原因之一是它对 x86 以外的体系结构的可移植性。

PE 格式代表 Windows 数据结构，详细说明了 Windows Loader 管理打包的可执行代码所需的信息，包括所需的动态库、API 导入和导出表等。

### Loader（加载器）

> [wiki/Loader_(computing)](https://en.wikipedia.org/wiki/Loader_(computing))

即：shellcode（单纯文本）的运行环境

> 在计算机系统中，加载器是操作系统的一部分，负责加载程序和库。它是启动程序过程中的重要阶段之一，因为它将程序放入内存并准备执行。加载程序涉及内存映射或将包含程序指令的可执行文件的内容复制到内存中，然后执行其他所需的准备任务以准备可执行文件的运行。加载完成后，操作系统将控制权传递给加载的程序代码来启动程序。

![[Pasted image 20240309154834.png]]

文件头： [https://www.garykessler.net/library/file_sigs.html](https://www.garykessler.net/library/file_sigs.html)



### DLL





### MZ

- PE的header/签名
- the legendary Mark Zbikowski



## 注册表

### 注册表的数据结构

注册表由项（也叫主键或键）、子项（子键）和值构成。
一个项就是分支中的一个文件夹，而子项就是这个文件夹当中的子文件夹，子项同样它也是一个项。
一个值则是一个项的当前定义，由名称、数据类型以及分配的值组成。
一个项可以有一个或多个值，每个值的名称各不相同，如果一个值的名称为空，则该值为该项的默认值。



- Collection of all settings and configurations that make the system run
- Made up of keys and values

### Root level keys

- HKEY_LOCAL_MACHINE (HKLM) - information on hardware and software
- HKEY_CLASSES_ROOT (HKCR) - information on file associates and OLE classes
- HKEY_CURRENT_USER (HKCU) - profile information for the current user including preferences
- HKEY_USERS (HKU) - specific user configuration information for all currently active users
- HKEY_CURRENT_CONFIG (HKCC) - pointer to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Hardware Profiles\Current

### Type of values

- REG_SZ - character string
- REG_EXPAND_SZ - expandable string value
- REG_BINARY - a binary value
- REG_DWORD - 32-bit unsigned integer
- REG_LINK - symbolic link to another key

### Important Locations

- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run

### Executables to edit

- regedit.exe
- regedt32.exe (preferred by Microsoft)




