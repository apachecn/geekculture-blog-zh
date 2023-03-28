# 为什么是莫奇托？

> 原文：<https://medium.com/geekculture/why-mockito-357850e2fe7d?source=collection_archive---------12----------------------->

## 测试工具

![](img/edbf210c9594fd2ca6cb4f3d5b4dbee1.png)

Photo by [Tolga Ulkan](https://unsplash.com/@tolga__?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

模拟的..PowerMock..

在为我们的代码库编写测试时，我们可能会遇到这些术语。在单元测试中，我们希望单独测试一个类的功能。但是一般来说，这些类不是独立的。一个类将调用另一个类的方法。所以，很难测试一个单独的类。

*   我们使用的一些外部服务在测试中可能无法正常工作…