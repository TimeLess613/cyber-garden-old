---
tags:
  - IT/Linux
---


## usr

> [!NOTE] User/Unix System Resource

- `/usr/bin`随系统更新
- `/usr/bin/local`的不会被更新覆盖

## passwd

- /etc/passwd ： 所有用户
	- 第二字段是密码：`x`表示存在shadow，`*` 表示无密码、即新用户、或停用该账号
- 而【/usr/bin/passwd】是改密需要执行的二进制程序文件。




## bashrc

> [!NOTE] rc：run command

- .bash_profile：只会在用户登录时运行。
- bashrc：每次启动新shell时都会运行（如执行bash这个命令）。可运行 `source .bashrc` 立即应用。
	- 用户级（~/.bashrc）覆盖系统级（/etc/bashrc）


## bin

- sbin：管理员命令、daemons
- /bin 被软连接到/usr/bin。相当于Windows的System32




## tmp

这个目录较特殊，其他用户对该目录的文件有可执行权限


## dev

- /dev 硬件驱动程序