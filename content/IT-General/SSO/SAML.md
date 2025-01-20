---
tags:
  - IT/认证
---
> Security Assertion Markup Language

SAML是一种SSO标准。

SAML协议的基石：公私密钥/公钥基盘，所以“服务提供商”都信任“身份提供商”。

- SAML 断言是 XML 格式的数据包，包含有关用户身份和属性的信息。它是一个被签名的安全令牌（所以基石为公钥基盘），用于在不同系统之间传递认证和授权信息。

主要用途有：实现云系统/WEB间的相互认证。

![[Pasted image 20240908141120.png]]

> https://milestone-of-se.nesuke.com/product/oss/saml/


## 3个角色

- IdP（Identity Provider，身份提供商）
- SP（Service Provider，服务提供商）：信任 IdP 并希望使用它进行身份验证的应用程序。
- Principal（即Client）：尝试通过 IdP 登录 SP 的用户


## 2种身份验证流程

用户访问SP的常见方式有两种：

1. SP initiated login。用户首先访问SP的网站。如果用户与 SP 没有活动会话，则用户将被重定向到 IdP 进行身份验证。成功登录后，用户将被重定向回 SP。
2. IdP initiated login。用户首先访问 IdP，然后会看到他们有权访问的 SP 列表。从该列表中选择 SP 后，他们将被重定向到该 SP。

### SP initiated Flow

也称作 HTTP Redirect/POST Binding。

- AAD用这种
- Redirect即GET请求，由于数据量大，URL有长度限制，以及安全问题，所以不用Redirect而使用POST
- RelayState：会话管理不用Cookie而用RelayState。


- SAML 不维护会话，因此 SP 需要为每个经过身份验证的用户维护会话。没有会话则重定向用户。
- 当用户单击“使用 SSO 登录”按钮时，SP 会生成一条名为“AuthnRequest”的 XML 消息

![[Pasted image 20240908141831.png]]


### IdP-initiated SSO (HTTP POST Binding)

- 在IdP界面选择SP时触发认证

![[Pasted image 20240908142120.png]]


## 发展

### ADFS

![[Pasted image 20240908142410.png]]

### 对与个人

SAML是针对公司等组织机构，而针对个人发展出了[[OAuth 2.0|OAuth]]和[[OpenID]]。

- [https://milestone-of-se.nesuke.com/sv-advanced/server-software/sso/](https://milestone-of-se.nesuke.com/sv-advanced/server-software/sso/)




## 相关概念

### Hybrid Azure AD join

指windows设备同时加入AD域（注册机器账号，使用Kerberos）和AAD（注册设备，使用SAML）。