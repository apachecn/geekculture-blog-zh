# 深入探究 Java 8 散列表

> 原文：<https://medium.com/geekculture/a-deep-dive-into-java-8-hashmap-a976aca22f9b?source=collection_archive---------3----------------------->

![](img/4aa1f1169759c588c97e07905de1b5e4.png)

HashMap 中的主要数据结构包括 Array、LinkedList 和[红黑树](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)(二叉查找树的高级版本)。本文深入探讨 Java 中 HashMap 的 *put()* 和 *get()* 函数，以更深入地了解 HashMap 的工作原理。

## 类图

在高层次上，HashMap 扩展了 *AbstractMap* 类，该类在 Java 中实现了 *Map* 接口。