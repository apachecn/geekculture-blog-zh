# 对 Spring WebFlux 的深入研究

> 原文：<https://medium.com/geekculture/an-in-depth-look-at-spring-webflux-4bcb23759d92?source=collection_archive---------2----------------------->

当处理大量并发用户时，传统的基于 Spring MVC 的应用程序倾向于使用大量线程并消耗大量资源。Servlet 3.1 在 Servlet 3.0 的基础上引入了非阻塞 IO (NIO)支持，但是，使用它需要偏离 Servlet API 的其余部分，在 Servlet API 中，契约是同步的(Servlet，Filter)或阻塞的(getParameter，getPart)。

除此之外，在 HTTP 层建立完全非阻塞契约的框架(如 Netty)的引入，成为开发