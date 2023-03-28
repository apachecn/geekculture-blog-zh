# 使用 Laravel 限制 IP 地址为的 JWT 令牌

> 原文：<https://medium.com/geekculture/restrict-jwt-token-with-ip-address-using-laravel-77ce5ae3671a?source=collection_archive---------8----------------------->

![](img/b50024245ac1cb2b13bf875449077d46.png)

Source: freepik

身份验证是任何 web 应用程序最重要的部分之一。处理移动应用程序和服务器端之间的身份验证可能很棘手，需要更安全的方法。最好的方法之一是使用 JSON Web Token (JWT)。

JWT 令牌的结构由报头、有效载荷和签名组成。基本上，服务器端只是用现有的声明创建令牌…