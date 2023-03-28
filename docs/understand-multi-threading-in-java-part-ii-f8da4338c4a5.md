# 理解 Java 中的多线程——第二部分

> 原文：<https://medium.com/geekculture/understand-multi-threading-in-java-part-ii-f8da4338c4a5?source=collection_archive---------7----------------------->

![](img/d131da4fa7446d89d82f8474bac19b77.png)

The photo was taken from [Creative Commons](https://burst.shopify.com/licenses/creative-commons)

在的[上一篇](https://wynnt3o.medium.com/understand-multi-threading-in-java-part-i-2a1884369e5e)中，我们已经了解了两种线程状态， **NEW** 和 **RUNNABLE** 。在本文中，我们将继续深入理解其余的线程状态。

## 线程生命周期

线程可以处于下列状态之一:

*   `[NEW](https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.State.html#NEW)`
    一个尚未启动的线程就是这种状态。