# 现代微服务通信

> 原文：<https://medium.com/geekculture/rsocket-modern-microservices-communication-25469a3351eb?source=collection_archive---------1----------------------->

![](img/a57d53b093de3cc370df60261df3f32b.png)

RSocket 是一种完全反应式双向二进制通信协议，它通过提供背压、多路复用和流式传输等多种功能，试图克服 HTTP1 和 HTTP2 的一些限制。

在整篇文章中，我们将探索 RSocket、协议、特性及其 Java 实现[https://github.com/rsocket/rsocket-java](https://github.com/rsocket/rsocket-java)。