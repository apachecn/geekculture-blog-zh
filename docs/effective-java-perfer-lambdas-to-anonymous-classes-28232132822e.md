# 有效的 Java！比匿名类更喜欢 Lambdas

> 原文：<https://medium.com/geekculture/effective-java-perfer-lambdas-to-anonymous-classes-28232132822e?source=collection_archive---------67----------------------->

这个新的章节将我们带到一个新的部分，关于一些新的 Java 特性，lambdas 和 streams。我是 lambdas 的忠实粉丝，我认为它是 Java 语言的一大亮点。我在开发中每天都使用 lambdas，这无疑使我的代码更加清晰。

在 lambdas 之前，接口和有时具有单一方法的抽象类会被用作*函数类型*。这些实例的实现将通过*匿名内部类*创建，并允许在 Java 代码中进行某种形式的函数式编程。让我们看一个例子: