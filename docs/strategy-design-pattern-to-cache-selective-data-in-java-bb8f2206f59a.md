# 使用这种设计模式在 Java 中缓存选择性数据

> 原文：<https://medium.com/geekculture/strategy-design-pattern-to-cache-selective-data-in-java-bb8f2206f59a?source=collection_archive---------5----------------------->

在我工作的这个冲刺阶段，我们的任务是构建一个新的项目 A，它需要重构一个现有的项目 B。项目 B 具有调用后端 Oracle 数据库的业务逻辑。经过调查，我们发现有几个表和字段是经常使用的。

然后我们决定通过使用**策略**设计模式来缓存这些数据。

> **策略**是一种行为设计模式，让你定义一系列算法，将它们放入一个单独的类，并使它们的对象…