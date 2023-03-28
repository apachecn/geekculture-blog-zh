# 如何在 EF Core 6 中使用带有时态表的历史数据

> 原文：<https://medium.com/geekculture/working-with-historical-data-using-temporal-tables-in-ef-core-6-5bf12719beac?source=collection_archive---------4----------------------->

## 使用 SQL Server 和实体框架核心直接存储、查询和恢复历史数据

![](img/8ae2ba3022293b5fccdfb110533b2989.png)

Photo by [Brooke Campbell](https://unsplash.com/@bcampbell?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

时间不仅存在于我们的生活中，也经常存在于我们的应用中。我们将带有时间戳的事件存储在数据库中，计划在特定时间点执行的操作…