# 有效的 Java:明智地使用本机方法

> 原文：<https://medium.com/geekculture/effective-java-use-native-methods-judiciously-efd3cb89241e?source=collection_archive---------58----------------------->

![](img/82989a514efe10c7a6ab42ac6979324f.png)

Photo by [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Java 提供了 Java 本地接口(JNI ),允许 Java 程序调用用 C、C++或 Rust 等本地编程语言编写的本地方法。从历史上看，这种能力用于三种不同的用途:

1.  能够调用特定于平台的特性，如注册表。
2.  访问现有的本机代码，这样您就不会…