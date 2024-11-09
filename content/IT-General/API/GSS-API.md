---
tags:
  - IT/认证
---
Generic Security Service Application Program Interface

一种统一模式，旨在为使用者提供一种无关平台环境、机制、程序语言的可移植的安全服务。程序员得以简单地开发通用的安全实现。

- 属于[[SASL]]框架的一种
	- IANA管理的SASL机制清单： [https://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml](https://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml)
- 不同于SASL是客户端专门针对一台服务器上（协商）的一种协议进行身份验证（参考[[SASL#SASL的工作方式]]），**GSS-API 是协议无关的**，因此可以使用一种身份验证行为并扩展到多种协议。（于是可以实现SSO）
- GSS-API也只是一个框架，所以需要具体实现。而使用GSS-API最有名的实现是：微软WindowsAD域的[[SPNEGO]]。
- [[NTLM]]和[[Kerberos]]都支持GSS-API，区别在于token。一个是NTLM token，一个是Kerberos ticket。
	- SSPI是GSS-API的一个变体，进行了扩展并具有许多特定于Windows的数据类型。
	![[Pasted image 20240816121836.png]]



## SASL与GSS-API的差异

![[Pasted image 20240816121943.png]]