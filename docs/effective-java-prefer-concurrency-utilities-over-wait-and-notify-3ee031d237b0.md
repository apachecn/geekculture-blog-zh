# 有效的 Java:比起等待和通知，更喜欢并发实用程序

> 原文：<https://medium.com/geekculture/effective-java-prefer-concurrency-utilities-over-wait-and-notify-3ee031d237b0?source=collection_archive---------9----------------------->

![](img/d9c50c828a5800b2f8cbe57151d19a88.png)

Photo by [Phil Hearing](https://unsplash.com/@philhearing?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Java 语言中每个对象的核心有三个方法，wait、notify 和 notifyAll。这些方法允许您使用低级并发控制选项。在 Java 5 之前，这是促进并发控制的首选。然而，自从 Java 5(2004 年)发布以来，现在有了更高级的工具可以使用，这些工具更加简单，也更少…