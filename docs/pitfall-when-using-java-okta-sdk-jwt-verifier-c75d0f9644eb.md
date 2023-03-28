# 使用 Java Okta SDK JWT 验证器时的陷阱

> 原文：<https://medium.com/geekculture/pitfall-when-using-java-okta-sdk-jwt-verifier-c75d0f9644eb?source=collection_archive---------7----------------------->

![](img/21a7f4e852c5e56744ac2db458316c07.png)

Photo by [Nick Abrams](https://unsplash.com/@nbabrams?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在今天的快速帖子中，我想覆盖一个我见过的一些开发人员在使用 Okta Java SDK 验证 JWT 签名时陷入的陷阱。我发现有趣的是，两个通过 Oauth2 集成 Okta 的独立团队在验证 jwt 时遇到了同样的问题。