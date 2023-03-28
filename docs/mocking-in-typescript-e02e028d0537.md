# 打字稿中的嘲讽

> 原文：<https://medium.com/geekculture/mocking-in-typescript-e02e028d0537?source=collection_archive---------5----------------------->

![](img/e99b2fd47f5f09503c7a279d39614d9b.png)

通过模拟不同的依赖行为，锁定是测试软件单元最简单的方法。以便您可以通过创建表示依赖关系的模拟对象来测试大多数正面和负面的情况。

今天我们要检查一个模仿库在 TypeScript 中的基本用法，这个库叫做`ts-mockito`，受 Java 中`Mockito`的启发。