# 有效的 Java:不要依赖线程调度器

> 原文：<https://medium.com/geekculture/effective-java-dont-depend-on-the-thread-scheduler-2435afa27550?source=collection_archive---------19----------------------->

![](img/e8b2b2ff3362bacf8c58c98e6f943897.png)

Photo by [Estée Janssens](https://unsplash.com/@esteejanssens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

即使在拥有许多 CPU 内核从而可以并发执行多个线程的现代系统上，它们也可能无法与系统中处于可运行状态的线程数量相匹配。出于这个原因，我们有*线程调度器*，它决定哪些线程将运行以及运行多长时间。线程调度器的实现在对待线程的方式上力求平等，但是它们的确切语义…