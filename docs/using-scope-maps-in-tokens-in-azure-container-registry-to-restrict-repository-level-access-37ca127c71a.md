# 在 Azure Container Registry 的令牌中使用范围映射来限制存储库级别的访问

> 原文：<https://medium.com/geekculture/using-scope-maps-in-tokens-in-azure-container-registry-to-restrict-repository-level-access-37ca127c71a?source=collection_archive---------14----------------------->

![](img/94656cd92f747c147a49aebd8b27668f.png)

Tom Fisk @Pexels

在这篇文章中，我想让大家知道我们如何利用这篇文章中的一个技巧，即存储库范围的权限。

我们现在可以为 ACR 创建更细粒度的权限。

*   限时访问有助于在特定时间点后阻止任何访问。