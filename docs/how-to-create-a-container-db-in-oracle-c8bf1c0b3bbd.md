# 如何在 Oracle 中创建容器数据库

> 原文：<https://medium.com/geekculture/how-to-create-a-container-db-in-oracle-c8bf1c0b3bbd?source=collection_archive---------16----------------------->

这里我们看到了在 oracle 中创建容器的步骤

安装 oracle 12c 软件后，让我们来看看如何在 Oracle 12c 中创建容器数据库

![](img/06cdb4e95920d34f0239b883b0f7dde9.png)

s-1:

转到$ORACEL_HOME/bin 位置给定

。/dbca

它将填充下面的屏幕，并选择“创建数据库”选项

![](img/2a55a4e245d568e7e6340b31564e860b.png)

s-2:

选择如下“高级选项”

![](img/98efa5e8ed15ad265de8c2e38b5bf882.png)

s-3

选择“通用单实例”选项

![](img/6a2df3b8bc802cf729620220ca8e6b8d.png)

s-4

我们需要提供实例名称，这里我们将创建一个空的容器数据库，因此选择下面的选项

![](img/71ef59431c15ba6eb1aefda223a34dfe.png)

s-5:

我们需要选择本地文件系统

![](img/d97bb25a167092ce3817299c87c8fc5b.png)

s-6:

我们通过选择此选项来启用 FRA 和归档日志模式

![](img/fa584fc7c72df28d03835714e6190d18.png)

s-7:

如果已经存在一个侦听器，我们可以选择它，或者为 DB wise 创建一个单独的侦听器，我们应该创建一个条目以及如下所示的端口

![](img/974cf02780fcb295d9ef1298013da0c3.png)

s-8:

我们现在不启用保管库和安全性，因为我们稍后将看到这一点，而不选择此选项继续下一步

![](img/4632c01fa0a0f6a464f392a3462acf6b.png)

s-9:

我们正在启用大小 AMM(自动内存管理)

![](img/9f66644ad5881243bd56a337edb5338c.png)

s-10:

我们不支持企业管理器表达

![](img/7e3de9084d1a14f67e581c1d5ba8b0c8.png)

s-11

设置 sys & system 口令，并按 yes 继续

![](img/f0b1a6be1a8a4233fe34d6ea8d71c043.png)

s-12

创建如下所示的数据库

![](img/da6fbc684424a5c4f2cb44a94b7c4e79.png)

安装完成后，连接数据库并检查它

![](img/670c27d580ac50bb0c59f7cfb3ccb9a0.png)