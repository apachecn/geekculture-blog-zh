# JVM 中字节码到类对象的转换一瞥

> 原文：<https://medium.com/geekculture/a-glance-on-conversion-from-bytecode-to-class-objects-in-jvm-e828559984d9?source=collection_archive---------12----------------------->

![](img/4aa1f1169759c588c97e07905de1b5e4.png)

> 作为 Java 开发人员，我们每天都要和类打交道。了解一个类如何在 JVM 中使用可以帮助我们更好地理解 Java 类。

首先，Java 类存储在字节码文件中。这些字节码文件经历了三个…