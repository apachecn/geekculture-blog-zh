# 使用 Clojure 抓取 web 产品数据

> 原文：<https://medium.com/geekculture/scraping-web-product-data-with-clojure-6594a86c2f00?source=collection_archive---------14----------------------->

站点地图，浏览器驱动，以及在 Mongo 中存储产品信息。

![](img/2c05f1ff9cc97951ff1c331477dcef5b.png)

Photo by [Brina Blum](https://unsplash.com/@brina_blum?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 背景

我需要获取一家公司的产品数据目录，觉得应该尝试一下网络抓取...事实证明，它在 Clojure 中实现起来简单、简洁、快速！在本帖中，我们将看看如何从 Bellroy 的网站上抓取所有产品。请注意，所有刮刀必须遵守…