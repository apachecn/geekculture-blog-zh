# 在容器中测试远程 OJDBC 连接

> 原文：<https://medium.com/geekculture/testing-a-remote-ojdbc-connection-within-a-container-aa297b2393cf?source=collection_archive---------12----------------------->

![](img/7d4e6bfd5dcfa28c42a3121dcbd1a73f.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## Oracle |数据库连接|映像| docker CLI

您是否有过这样的经历:一个容器需要连接到一个 oracle 数据库，但是失败了，并且花了几个小时试图找出原因？我也是。这就是我写这篇文章的原因。希望能帮某人争取回时间。所以让我们开始吧。