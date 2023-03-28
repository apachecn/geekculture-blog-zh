# Spring Boot 的实时用户跟踪

> 原文：<https://medium.com/geekculture/realtime-user-tracking-in-spring-boot-d8fb343a6c3c?source=collection_archive---------4----------------------->

在本文中，我们将构建一个端到端的示例，从管理页面查看所有登录的用户。每当有人登录时，一个指示板应该会实时反映这一变化，而不需要管理员刷新页面。我们还将监控用户登录失败的次数。这可能有助于检测一些暴力攻击。理想情况下，您已经想到添加一个[锁定机制](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism)。

在这种端到端实施中，我们需要:

*   登录页面——为了简单起见，我们将使用默认的 [spring security](/javarevisited/top-10-courses-to-learn-spring-security-and-oauth2-with-spring-boot-for-java-developers-8f0222d6066d?source=---------5-----------------------) 表单登录页面。我们的…