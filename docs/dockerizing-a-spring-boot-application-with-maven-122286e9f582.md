# 用 Maven 编写 Spring Boot 应用程序的文档

> 原文：<https://medium.com/geekculture/dockerizing-a-spring-boot-application-with-maven-122286e9f582?source=collection_archive---------6----------------------->

在之前的文章中，我们创建了在本地运行的 T2 Spring Boot REST API。

在创建 Dockerfile 之前，我们需要修改 pom.xml 文件，以包含将在构建生命周期中执行的构建插件。我们需要它来生成可执行的 jar 文件。