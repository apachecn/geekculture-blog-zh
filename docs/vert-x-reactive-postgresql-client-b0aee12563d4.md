# Vert.x -反应式 PostgreSQL 客户端

> 原文：<https://medium.com/geekculture/vert-x-reactive-postgresql-client-b0aee12563d4?source=collection_archive---------5----------------------->

## 数据持久性—反应式方法

Vert.x 为 PostgreSQL 提供了一个反应式客户端，它有一个简单明了的 API，专注于可伸缩性和低开销。

![](img/a2fbb7e8da4c3a6b701fcdb259c9245e.png)

客户端是反应性的和非阻塞的，允许使用通过 Netty 实现的单线程来处理许多数据库连接。