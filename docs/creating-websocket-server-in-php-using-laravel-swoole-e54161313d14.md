# 使用 Laravel & Swoole 在 PHP 中创建 WebSocket 服务器

> 原文：<https://medium.com/geekculture/creating-websocket-server-in-php-using-laravel-swoole-e54161313d14?source=collection_archive---------2----------------------->

![](img/733281cdfb85e47bad39c0246957255d.png)

Photo by [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

W ebSocket 是客户端&服务器之间的双向 TCP 通信协议。与 HTTP 不同的是 WebSocket 可以建立&保持客户端和服务器之间的通信，直到其中一方终止通信。而 HTTP 协议不这样做。HTTP 并不维护通信，所以每当客户端发出请求时，连接就会…