---
tags:
  - IT/认证
---


> OAuth 2.0 是应用程序用来为客户端应用程序提供访问权限的标准。
- 即主要用于授权。

OAuth 2.0 使用访问令牌（Access Token）来代表用户的授权。访问令牌通常是简短的字符串，由认证服务器签发，并且用于访问资源服务器上的受保护资源。


```
 +--------+                               +---------------+
 |        |--(A)- Authorization Request ->|   Resource    |
 |        |                               |     Owner     |
 |        |<-(B)-- Authorization Grant ---|               |
 |        |                               +---------------+
 |        |
 |        |                               +---------------+
 |        |--(C)-- Authorization Code --->| Authorization |
 | Client |                               |     Server    |
 |        |<-(D)----- Access Token -------|               |
 |        |                               +---------------+
 |        |
 |        |                               +---------------+
 |        |--(E)----- Access Token ------>|    Resource   |
 |        |                               |     Server    |
 |        |<-(F)---- Protected Resource --|               |
 +--------+                               +---------------+
 
 A: 客户端向资源所有者（用户）请求授权，并被重定向到授权服务器。
 B: 资源所有者同意授权，并将授权授予客户端（通常通过授权码）。
 C: 客户端使用授权码向授权服务器请求访问令牌。
 D: 授权服务器验证授权码并返回访问令牌。
 E: 客户端使用访问令牌向资源服务器请求受保护的资源。
 F: 资源服务器验证访问令牌并返回受保护的资源数据。
```

> [!note] 根据kazkiti.ctf OAUTH-004的经验，请求时有几个编码问题需要注意。


---

## 3种认证方式

ServiceNow的参考连接： [OAuth Grant Types: Explained](https://support.servicenow.com/kb?id=kb_article_view&sysparm_article=KB1647747)