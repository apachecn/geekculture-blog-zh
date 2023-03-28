# EF 迁移与 SQL Dacpac

> 原文：<https://medium.com/geekculture/ef-migration-vs-sql-dacpac-a15d68184481?source=collection_archive---------2----------------------->

在一些项目中，我会提出这样的问题:“我们如何才能让你的数据库保持最新？”。因此，有不同的方法，我将选择两种流行的方法，为部署迁移提供良好的数据库工具(由实体框架和 SAL Dacpac 部署支持)。

![](img/a700a50eccf27ac706c2d451b0945ab7.png)

[Sergei Starostin](https://www.pexels.com/de-de/@sergei-starostin-15862958/) @ pexels

让我们对照这些标准来分析这两种方法

*   设置
*   可维护性