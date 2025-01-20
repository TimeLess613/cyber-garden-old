---
tags:
  - IT/网络
  - IT/加密
---

> https://milestone-of-se.nesuke.com/sv-advanced/digicert/digital-certification-summary/


## SSL握手

SSL在TCP和应用层之间，保证了传输层的安全。


> [!note] 总结：Hello/协商、证书交换、密钥交换


## 举例：HTTPS通信步骤

### 一、协商加密组件&提供公钥证书

1. 客户端Hello，提供可选[[加密组件]]。
2. 服务器回应加密组件，并提供[[公钥证书]]。

### 二、验证公钥证书&共享pre-master secret

3. 客户端验证公钥证书，没问题则用服务器的公钥加密pre-master，并（提前?）发送Change Cipher Spec（更改密码规格）报文，告诉服务器之后用pre-master密钥（服务器可用私钥解密出master密钥——session key）、对称加密进行通信

- 即，SSL会话key是在验证完服务器的身份后，由客户端生成

### 三、（校验）SSL建立完毕之后，可发送HTTP请求/响应

### 四、断开连接时进行TCP四次挥手


