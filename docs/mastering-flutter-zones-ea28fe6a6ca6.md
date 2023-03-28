# 掌握颤振区域

> 原文：<https://medium.com/geekculture/mastering-flutter-zones-ea28fe6a6ca6?source=collection_archive---------3----------------------->

![](img/238f837ce31b23642797196faa531ab5.png)

为什么我们需要颤振区域？因为发现一个被吞掉的 app 异常真的很头疼。

一个例子，从应用程序的主函数根中省去这个:

**widgetsflutterbinding . ensure initialized()；**

**现在引用一个目标设备，在 Flutter Sky 引擎加载之前尝试引用设备上的一个文件路径。是的，你猜对了；你得到一个** …