# 编写定制的梅林 C2 模块

> 原文：<https://medium.com/geekculture/building-custom-merlin-c2-modules-a8183c32b660?source=collection_archive---------13----------------------->

## 自动化开采后操作

![](img/06881b09b9c03f9a4b21e4cf0f014d0d.png)

你好，世界！Merlin 是一个现代的 C2 框架，具有许多特性，比如支持 HTTP/2 协议、内置的 JWT 认证、域前置等等。Merlin 是用 Go 编写的，这意味着我们可以很容易地用一个 CPU 在大多数事情上部署 Merlin 服务器和代理。Merlin 附带了一组模块，用于执行常见的…