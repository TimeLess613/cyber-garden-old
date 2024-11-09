---
tags:
  - IT/Windows
---
## System32

**在64位Windows上`System32`包含64位系统文件。直接支持64位操作系统的运行。**
在32位Windows上`System32`就包含32位系统文件。

## SysWOW64

`SysWOW64`的"WOW"代表"Windows 32-bit on Windows 64-bit"。
即它是一个兼容层，供在64位系统上运行的32位应用程序使用，所以里面**包含32位系统文件**。
所以**仅64位系统有**，32位系统没有。


> [!NOTE] 即仅64位系统要考虑
> - 64位系统要跑64bit程序时，找32文件夹——因为它直接支持64位操作系统的运行。
> - 64位系统要跑32bit程序时，找64文件夹——因为里面放着32bit用的文件。



## 判断应用程序的位数

#渗透/持久化  （为了立足shell的稳定性等）

- `[System.Environment]::Is64BitProcess`
- `%windir%\syswow64\WindowsPowerShell\v1.0\powershell.exe "[IntPtr]::size"`

`if([IntPtr]::Size -eq 4)`
- IntPtr.size：指针类型的字节数、内存地址的长度
	- 32位的size：32bit=**4** Byte
	- 64位的size：64bit=**8** Byte

### 不同位数powershell的路径

- 64位powershell（在`System32`）：`%windir%\System32\WindowsPowerShell\v1.0\powershell.exe`
- 32位powershell（在`syswow64`）：`%windir%\syswow64\WindowsPowerShell\v1.0\powershell.exe`





## Sysnative

[https://www.samlogic.net/articles/sysnative-folder-64-bit-windows.htm](https://www.samlogic.net/articles/sysnative-folder-64-bit-windows.htm)

`Sysnative`并不是一个实际存在的文件夹，你无法在文件资源管理器中找到它。
它仅仅是一个从32位应用程序到64位`System32`目录的虚拟视图，使得这些应用程序可以调用64位的系统工具或文件。

### Sysnative的作用

- **访问64位系统文件**：`Sysnative`提供了一种机制，允许运行在Windows 64位系统上的32位应用程序访问`System32`文件夹中的64位系统文件。如果没有这种机制，这些32位应用程序默认会被重定向到`SysWOW64`文件夹，从而只能访问32位的系统文件。
- **解决兼容性问题**：通过使用`Sysnative`路径，软件开发者可以确保他们的32位应用程序能够正确地与64位系统组件交互，解决因文件系统重定向可能导致的兼容性问题。
