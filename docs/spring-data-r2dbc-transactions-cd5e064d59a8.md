# 春季数据 R2DBC -交易

> 原文：<https://medium.com/geekculture/spring-data-r2dbc-transactions-cd5e064d59a8?source=collection_archive---------0----------------------->

# 介绍

在 Spring 中，R2DBC 是反应式事务的集成之一。与事务状态与线程相关联的传统事务相比，反应式事务通过反应器上下文来管理。除了介绍 Spring 框架内的反应式事务，本文还提供了关于 Spring Boot 魔术如何在幕后工作的扎实知识。

我强烈建议在阅读本文之前先了解一下 Reactor 的背景。

# JDK 代理快速浏览