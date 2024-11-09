---
tags:
  - IT/Windows
  - 渗透
---
## 链接

> [hacktricks: Basic Win CMD for Pentesters](https://book.hacktricks.xyz/windows-hardening/basic-cmd-for-pentesters)
> [ffffffff0x 团队：Speed-Win](https://github.com/ffffffff0x/1earn/blob/master/1earn/Integrated/Windows/Speed-Win.md)



## 环境变量

- 查看所有：`set`
- 查看指定变量：用百分号包围指定变量，如：`echo %userprofile%`

> [[powershell#环境变量]]


## DNS

语法：`nslookup -qt=类型 目标域名 指定的DNS服务器IP或域名`

例子：`nslookup -qt=A tool.chinaz.com 8.8.8.8`



## cmdkey

> [Windows Credential Manager for hackers](https://systemweakness.com/windows-credential-manager-for-hackers-e67aad9c2a75)

cmdkey 命令允许列出、添加和删除凭据管理器的凭据。
![[Pasted image 20240309144240.png]]
![[Pasted image 20240309144023.png]]

- 典型的利用工具：runas /savecred（使用储存的凭据）
	- 注意：Windows 图形界面中 Runas 的等效版本是按住 Shift 键并右键单击程序图标（例如 cmd.exe）。我们将在上下文菜单中看到一个新选项“以其他用户身份运行”。然后我们将被要求提供凭据。这与使用 Runas 以不同于当前用户的用户身份运行程序是一样的。

```bash
## Create the "test" local user and give it a password "test"
net user test /add
net user test test

## Create a credential called "test1" for this user
cmdkey /add:test1 /user:machine\test /pass:test

## The Runas command works the same if we create the credential with /generic instead of /add:
cmdkey /generic:test1 /user:machine\test /pass:test

## Run cmd as this user, using the stored credential that we just created
runas /user:machine\test /savecred cmd

## Same as above, but don't use the stored password. Must be inserted interactively
runas /user:machine\allc /password cmd.exe 
```



[域用户似乎runas第一次使用储存的凭据会要求输入密码](https://systemweakness.com/runas-for-hackers-6b2243633091)
- 那么为什么会保存在这？因为多人用这个机器？


## klist

显示当前用户缓存的所有Kerberos凭据。

> klist 命令仅显示当前用户的缓存票证。可能存在其他票证，属于其他用户（并且始终属于机器帐户），但 klist 不显示它们。要获得它们，我们必须使用 Mimikatz 等工具转储票据。

[Playing with Kerberos tickets (Host service)](https://systemweakness.com/playing-with-kerberos-tickets-host-service-7c2fe3dcb0e0)


## tasklist

显示（非特权用户的）进程（即任务管理器-进程）

- `tasklist /svc`：显示各个进程的服务


## 计划任务

### schtasks

`schtasks /query [/tn vulntask_full_path] /fo list /v`
- tn默认根路径，其他路径的任务需要指定路径
### at

旧版



## net命令

[https://zhuanlan.zhihu.com/p/413911061](https://zhuanlan.zhihu.com/p/413911061)

### net view：查看可连接的资源

### net user

- net user：枚举所有本地账户
- net user /domain [域账户(不用写域名)]：枚举域环境的所有（或指定）用户

### net localgroup（本地组）

### net group（Windows Server命令）

- net group /domain：枚举域环境中所有的组。（不会列出嵌套组）

### net share

### net use

- 无参数时即查看连接
- 可映射远程共享（share）到本地磁盘（device，如"F:"等，不分大小写）；也可进行Deviceless连接，之后可直接用命令查看远程共享（如：`dir \\SV\c$`）

> 如果您的客户端计算机和远程Windows服务器位于ActiveDirectory域中，并且您使用域用户帐户登录到您的客户端计算机，那么在这种情况下使用`net use`通常没有意义。
> 您已经可以访问这些资源而无需额外的步骤。但是，当您需要使用备用用户帐户进行身份验证时，这就是它派上用场的地方。


删除IPC连接：`net use \\server\ipc$ /del`
域内主机：`net use \\server ip\ipc$ "password" /user:domain\username`
查看目标的文件列表：`dir \\server ip\c$\`


### net start

显示已启动的服务（服务：类比Linux的守护进程）


### net account/domain

查看密码policy



## nbtstat

```shell
>nbtstat /?

显示协议统计和当前使用 NBI 的 TCP/IP 连接
(在 TCP/IP 上的 NetBIOS)。

NBTSTAT [ [-a RemoteName] [-A IP address] [-c] [-n]
        [-r] [-R] [-RR] [-s] [-S] [interval] ]

  -a   (适配器状态)    列出指定名称的远程机器的名称表
  -A   (适配器状态)    列出指定 IP 地址的远程机器的名称表。
  -c   (缓存)          列出远程[计算机]名称及其 IP 地址的 NBT 缓存
  -n   (名称)          列出本地 NetBIOS 名称。
  -r   (已解析)        列出通过广播和经由 WINS 解析的名称
  -R   (重新加载)      清除和重新加载远程缓存名称表
  -S   (会话)          列出具有目标 IP 地址的会话表
  -s   (会话)          列出将目标 IP 地址转换成计算机 NETBIOS 名称的会话表。
  -RR  (释放刷新)      将名称释放包发送到 WINS，然后启动刷新

  RemoteName   远程主机计算机名。
  IP address   用点分隔的十进制表示的 IP 地址。
  interval     重新显示选定的统计、每次显示之间暂停的间隔秒数。
               按 Ctrl+C 停止重新显示统计。
```


## 查找文件

`where`查找文件

`find`和`findstr`是找文件中的字符串

findstr
- 不带参数：就是当成正则匹配来搜索
- /C:  ——  相当于fgrep



## 查看eventlog审查policy

`auditpol /get /category:*`


## icacls

`icacls`直接查看目标的DACL

`icacls C:\example.txt /save C:\output.txt`可以看SACL

## sc