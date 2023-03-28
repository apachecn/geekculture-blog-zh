# 变更数据捕获—使用 Debezium

> 原文：<https://medium.com/geekculture/change-data-capture-using-debezium-ec48631d643a?source=collection_archive---------10----------------------->

本文将帮助您集成 Debezium 来捕获数据事件中的变化，还可以帮助您确定分解整体应用程序的方法。

![](img/d53ed75d3a86c421c7daf5281e16284d.png)

Photo by [Margaret Weir](https://unsplash.com/@margotd1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我写这篇文章是为了解释我最近在为我们的一个客户将一个单一的应用程序分解成基于事件驱动架构的微服务应用程序时遇到的问题。