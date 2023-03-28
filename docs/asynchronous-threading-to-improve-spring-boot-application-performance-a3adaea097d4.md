# 提高 Spring Boot 应用程序性能的异步线程

> 原文：<https://medium.com/geekculture/asynchronous-threading-to-improve-spring-boot-application-performance-a3adaea097d4?source=collection_archive---------0----------------------->

## 方案

假设您有一个请求用户信息的 Spring 应用程序。有 200 个用户向 web 服务器发送请求。每个请求都被转发到后端服务以检索用户信息。当第一个线程到达 web 服务器中的 Tomcat 线程时，它调用后端方法并等待它返回结果。然后，它将结果转发给服务器，并返回给用户。其他请求必须等待第一个请求完成后才能被处理。如果每个…