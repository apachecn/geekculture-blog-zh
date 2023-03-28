# 微服务架构上 API 网关中的 JWT 认证过程

> 原文：<https://medium.com/geekculture/how-jwt-is-implemented-in-api-gateway-on-microservice-architecture-5dce8f5b89aa?source=collection_archive---------0----------------------->

## 微服务| JWT

![](img/351ab66f42b8e9852e1226bb74c53df5.png)

Photo by [dylan nolte](https://unsplash.com/@dylan_nolte?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/2003981/microservices?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在微服务架构中，一个应用被分成多个微服务。例如，amazon 应用程序可以分为订单、目录、用户信息、评级、评论、类别、购物车和其他不同的微服务。

还有。大多数情况下，这些服务位于安全墙之后，用户使用安全逻辑进行身份验证。这是什么意思？

假设我们需要将某个产品添加到购物车中，但是如果不经过登录(身份验证)过程，我们就无法做到这一点。每当我们希望调用微服务来处理客户端请求时，都需要在每个服务中实现这个安全逻辑。但是，每次调用微服务时，让每个微服务承担验证用户的开销是低效的。

为了解决这个问题，使用基于令牌的认证将客户端请求认证到 API 网关。

我们将逐步讨论基于令牌的身份验证过程。为了更好地理解，请使用下图。尽管步骤 7 没有在该图像中示出。

![](img/12c394c93ed309789e34c13bdab95c1a.png)

JWT Implementation in Microservices, Graphics by [SPSarkar](/@spsarkar-appxive)

1.  用户将其凭证提交给**身份服务器/授权服务器**以获得参考访问令牌。
2.  身份服务器/授权服务器验证用户凭证。对于合法用户，服务器存储令牌的内容并生成一个**参考访问令牌**，服务器将该令牌发送给客户端。这个令牌是随机加密的。
3.  客户端接收访问令牌，并将**请求+参考访问令牌**发送给 API 网关。
4.  **API 网关**从客户端接收令牌，并再次将接收到的访问令牌发送给身份服务器/授权服务器。
5.  **身份服务器/授权服务器**验证访问令牌，并向 API 网关返回 **JSON Web 令牌(JWT)** 。现在这个 **JWT 包含用户和许可信息。**

> JSON Web Token (JWT)是一个开放标准( [RFC 7519](https://tools.ietf.org/html/rfc7519) )，它定义了一种紧凑且独立的方式，以 JSON 对象的形式在各方之间安全地传输信息。—[https://jwt.io/](https://jwt.io/)

6. **API 网关**将客户端请求发送给相应的微服务，微服务可以与 JWT 一起处理**客户端请求。**

7.现在，微服务通过使用 JWT 的内容来检查身份验证和授权。JWT 有三个部分，用一个点(.)即报头、有效载荷和签名。

```
eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiSm9lIENvZGVyIn0.5dlp7GmziL2QS06sZgK4mtaqv0_xX4oFUuTDh1zHK4U #JWT (header.payload.signature)# ------------------------*# header (hashing algorithm and type of token)*
*# payload (authorization information and expiry of token)*
*# signature (refers to the header & payload, which is digitally signed with the server's key)*
```

现在微服务提取**头**和**有效载荷**，**计算这两个 Base64-URL 编码的 JSON 的 hash** 。现在微服务用服务器的密钥解密密码**签名**部分，签名实际上是报头和有效载荷的散列。现在，微服务检查两个哈希值是否相等，如果两个哈希值相同，则服务将断定客户端是经过身份验证的客户端。

## 微服务中基于令牌的认证有什么优势？

主要优势是减少每次调用微服务时验证用户的开销。因为 **JWT 在其有效载荷中包含用户身份信息。微服务使用 JWT 来执行身份验证和授权，从而避免执行额外的操作，如数据库查询来检查用户身份验证。**

这个帖子对你有帮助吗？不要忘记为这个帖子鼓掌，给我们一些灵感！

# 你对全栈开发感兴趣吗？

*我写的是关于 fullstack Web 开发 REST API、微服务、LinkedIn 中的架构、Medium 的循序渐进的编码教程。* [*这里是我的 Linkedin 简介*](https://www.linkedin.com/in/spsarkar-appxive/) *。*