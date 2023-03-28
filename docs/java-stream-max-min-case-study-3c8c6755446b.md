# Java 流:最大(最小)—案例研究

> 原文：<https://medium.com/geekculture/java-stream-max-min-case-study-3c8c6755446b?source=collection_archive---------11----------------------->

![](img/c1e5a91639511712eefbc3351615b5fe.png)

What a Beautiful Stream! Photo by [Sprocket2cog](https://commons.wikimedia.org/w/index.php?title=User:Sprocket2cog&action=edit&redlink=1) from [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Urban_stream_in_park.jpg)

## **最大**和最小终端操作

## 一个特例，对现实生活中的问题非常有用

`**max**()`和`**min**()`是 ***终端操作*** 来自 **Java Stream API** 库。

一个**终端操作**，比如一个`**collect**()`，终止**流**，返回不同的东西(一个列表，一个地图，一个可选的，一个…