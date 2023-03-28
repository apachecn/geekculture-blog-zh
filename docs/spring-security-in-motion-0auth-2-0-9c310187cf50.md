# spring Security in Motion-0Auth 2.0

> 原文：<https://medium.com/geekculture/spring-security-in-motion-0auth-2-0-9c310187cf50?source=collection_archive---------3----------------------->

在[的前一部分](/geekculture/spring-security-in-motion-part-1-8fc2644244f)中，我们深入探讨了 spring 安全组件，讨论了通过安全配置器配置安全过滤器的几种方法。在这一部分，我们将继续我们的旅程，并讨论如何配置我们的 http 过滤器链，以支持 OAuth 2.0 资源服务器。基本上，它包括添加一个过滤器来解析对承载令牌的请求，并通过检查接收到的令牌进行身份验证尝试。

我们还将学习如何配置 OAuth 2.0 登录，它利用了 OAuth 2.0 授权代码授权流，我们可以…