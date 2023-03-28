# 使 MVC 变得不那么庞大，更具可重用性

> 原文：<https://medium.com/geekculture/making-mvc-less-massive-and-more-reusable-cf731154ffbe?source=collection_archive---------11----------------------->

## 单一责任原则在起作用

## 实现可重用的编程 UICollectionView

![](img/f909c855b80416b186b0211e0156d4a9.png)

Your ViewControllers don’t have to be so massive

在本教程中，我们将以一种你可能不熟悉的方式实现 MVC。我们将创建一个可重用的`UICollectionView`,由它的父`UIView`实现，并将项目选择事件委托给一个…