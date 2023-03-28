# 有效的 Java:更喜欢原始类型而不是装箱类型

> 原文：<https://medium.com/geekculture/effective-java-prefer-primitive-types-to-boxed-types-ea02eb9b8c62?source=collection_archive---------9----------------------->

![](img/6303d09a9f6babbbefeb44890aee64cf.png)

Photo by [Christopher Bill](https://unsplash.com/@umbra_media?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Java 中的类型有两种风格，*原语*类型( *int* 、 *long* 等)和*引用*类型(*字符串*、*列表*等)。每个基元类型都有一个对应的引用类型，称为装箱基元。Java 确实有自动装箱和取消装箱功能，它们可以抽象出这些类型之间的区别，但并不完全像我们将要看到的那样。让我们来看几个例子，看看我们在使用…时会遇到什么麻烦