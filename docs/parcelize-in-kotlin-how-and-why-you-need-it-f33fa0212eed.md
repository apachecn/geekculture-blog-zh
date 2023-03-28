# 在科特林打包。你如何以及为什么需要它？

> 原文：<https://medium.com/geekculture/parcelize-in-kotlin-how-and-why-you-need-it-f33fa0212eed?source=collection_archive---------5----------------------->

由于对 KOTLIN 和 Java 不太熟悉，我总是觉得 parcelize 很有趣。希望在阅读完这篇文章后，您能对这个特性有更多的了解。

![](img/2b50cc5f33222620dbf36e53bcfa0356.png)

[link](https://pbs.twimg.com/media/E0aXaYeXMA4vira.jpg)

在 Android 上，`Parcelable`是一个类可以实现的接口，在一个意图内从一个活动传递到另一个活动，这样，从一个活动到另一个活动传输数据。