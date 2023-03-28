# Docker —存储驱动程序

> 原文：<https://medium.com/geekculture/docker-storage-driver-667c57169379?source=collection_archive---------8----------------------->

## 每天一点集装箱知识！

![](img/bdeb8c6bf286c8e19173c70a7a93d92a.png)

在这篇文章中，我将谈论 Docker 存储。Docker 为容器提供了两种资源来存储数据:

*   由存储驱动程序管理的图像层和容器层
*   数据卷宗

理想情况下，很少的数据被写入容器的可写层，并且您使用 Docker 卷来…