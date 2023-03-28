# 在 Spring Boot 与 Feign 客户端的微服务通信

> 原文：<https://medium.com/geekculture/microservices-communication-in-spring-boot-with-feign-client-182bcf1d355b?source=collection_archive---------3----------------------->

## 用 HTTP 协议通信两个微服务

![](img/b8028416403fa5532add38087a66cb3f.png)

Source: [https://unsplash.com](https://unsplash.com/)

微服务是从领域驱动设计的世界中出现的架构模式之一，它的许多应用程序相互通信，形成一个完整的应用程序。为此，常用的协议是 Http/Https。在本文结束时，您将学习如何使用 Feign——一个由网飞开发的声明式 HTTP 客户端——在 Spring Boot 应用程序中与微服务通信。

我假设您已经有了两个微服务(如果没有，您会在下面找到源代码)，所以我将直接跳到实现。对于这个例子，我有两个微服务(一个是 **UserServiceApp** ，第二个是 **ItemServiceApp** )。

我将创建一个服务来调用 **ItemServiceApp** 中的用户项目列表。

要使用 Feign，我们需要添加所需的依赖项:

最新版本，查看 [*这里*](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-openfeign) *。*

然后在主 **Application.class** 中添加***@ EnableFeignClients***注释

在**itemserviceapp**中，有一个 API 通过用户 ID 获取一个项目列表。

现在，为了在**用户服务应用**中使用它，我们需要一个接口服务，并添加 ***@FeignClient*** 注释，该注释带有在**应用程序.属性**中定义的微服务的名称

***@GetMapping*** 中的路径值是**itemserviceapp**中 API 的路径。

该服务将如下所示:

现在，我们可以调用 **UserService** 中的***getItemByUserId()***方法。

仅此而已，没那么复杂，对吧！

如果你面临任何问题，请写在评论里。

**资源:**

【https://spring.io/projects/spring-cloud-openfeign 

源代码: [***此处***](https://github.com/sabbarmehdi/microservices/tree/master/Micro-services)