# 如何在您的 SaaS 应用程序中设计特定于租户的子域

> 原文：<https://medium.com/geekculture/how-to-design-tenant-specific-subdomains-in-your-saas-application-5bcc19a54824?source=collection_archive---------2----------------------->

我最近在我们的产品中开发了一个配置特定于租户的自定义子域的功能。在这篇文章中，我分享了我们团队迭代的一些设计，以及它们的优缺点。

# 什么是特定于租户的子域？

特定于租户的子域(也称为自定义子域)是多租户 SAAS 解决方案中提供的一个常见功能，允许每个租户配置自己选择的子域。