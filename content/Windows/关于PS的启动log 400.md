---
tags:
  - IT/蓝队
  - IT/Windows
---
命令原文在HostApplication，而不是CommandLine。
- cmd启动：HostApplication=powershell.exe。（如果是-c调用，整行命令都会写在这）
- CTRL+R或Q搜powershell启动：`HostApplication=%windri%\WindowsPowerShell\v1.0\powershell.exe`
	- ps命令里的子进程"$()"似乎没有400log（不过父进程的log被记录了所以ok）