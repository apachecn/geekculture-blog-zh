# Django 性能:使用 RAM，而不是 DB

> 原文：<https://medium.com/geekculture/django-performance-use-ram-not-db-2f80c19c1b73?source=collection_archive---------3----------------------->

## 这种方法让你的应用程序快 10 倍！

这不是另一篇关于缓存的文章，尽管原理是相同的。尤其是如果您正在处理一个分布式系统架构，并且数据库在另一台机器上。

通常，项目的性能瓶颈是数据库。数据库查询本身很快(如果您设置了索引)，但是当您查询某些东西时会发生这种情况(至少在 Django 中是这样):