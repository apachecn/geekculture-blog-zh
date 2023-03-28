# REST API 最佳实践——将长期运行的任务从 HTTP 请求处理中分离出来

> 原文：<https://medium.com/geekculture/rest-api-best-practices-decouple-long-running-tasks-from-http-request-processing-9fab2921ace8?source=collection_archive---------1----------------------->

## 第 1 部分:讨论如何按照微软关于 ASP.NET 核心性能最佳实践的建议，在 RESP API 中设计和完成 HTTP 请求之外的长期运行任务。

![](img/2c22689f41cbb9aa445a747bd807520b.png)

Photo credit to Mike van den Bos on unsplash.com

## 背景