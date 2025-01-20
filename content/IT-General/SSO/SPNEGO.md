---
tags:
  - IT/Windows
  - 渗透/内网
  - IT/认证
---
Simple and Protected gss-api NEGOtiation mechanism


微软创建的SSO协议。在 [[GSS-API]] 之上运行。

即NegotiateSSP（ref: [https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc772815(v=ws.10)](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc772815(v=ws.10))）

- Location: `%Windir%\System32\lsasrv.dll`


## 統合 Windows 認証

集成 Windows 身份验证是 http(s) 的一项 SSO 功能，允许经过域身份验证的用户使用 Kerberos 服务票证以其权限登录，而无需在访问 Web 服务器时再次进行身份验证。

- 401响应报头：`WWW-Authenticate: Negotiate`
- 客户端浏览器返回嵌入ST的SPNEGO格式的KRB_AP_REQ，base64编码的Authenticate报头。

![[Pasted image 20240816123842.png]]





## 发展

近年来Web应用程序和云的高速发展，SSO也多应用于不同域之间。（如[[SAML]]）