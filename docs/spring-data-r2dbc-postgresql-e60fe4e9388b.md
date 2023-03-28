# Spring Data R2DBC [PostgreSQL]

> 原文：<https://medium.com/geekculture/spring-data-r2dbc-postgresql-e60fe4e9388b?source=collection_archive---------2----------------------->

[R2DBC](https://r2dbc.io/) (反应式关系数据库连接)是一种反应式 API 开放规范，为数据库驱动程序建立服务提供者接口(SPI)。它允许驱动程序提供与数据库的完全反应式无阻塞集成，这意味着，它使您能够从传统的每个连接一个线程的模式转移到更强大和可扩展的方法。查询被写入套接字，然后线程可以继续处理其他内容，直到收到响应，从而减少资源开销。