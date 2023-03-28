# Spring Boot 和 JWT 的基于角色的访问控制(RBAC)

> 原文：<https://medium.com/geekculture/role-based-access-control-rbac-with-spring-boot-and-jwt-bc20a8c51c15?source=collection_archive---------0----------------------->

## 使用 Spring Security 内置的 OAuth2 资源服务器，为基于角色的访问控制授予权限和方法安全性

![](img/7c14493dbc500ffff0564bd630f858f4.png)

Image by [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=511714) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=511714)

# 概观

大约一年前，我写过如何使用 [Spring Boot](https://spring.io/projects/spring-boot) 的内置 [**OAuth2 资源服务器**](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html) 保护你的 [**SPA**](/@NeotericEU/single-page-application-vs-multiple-page-application-2591588efe58) (单…