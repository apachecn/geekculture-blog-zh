# Java 工厂设计模式的案例研究

> 原文：<https://medium.com/geekculture/a-case-study-into-factory-design-pattern-in-java-49613e3fb078?source=collection_archive---------12----------------------->

![](img/354ce0fae2e36332a31c14be9dc67c67.png)

> **工厂方法模式**是一个[创建模式](https://en.wikipedia.org/wiki/Creational_pattern)，它使用工厂方法来处理[创建对象](https://en.wikipedia.org/wiki/Object_creation)的问题，而不必指定将要创建的对象的确切[类](https://en.wikipedia.org/wiki/Class_(computer_programming))。
> -维基百科

假设我们有一个 *OrderService* 类。这个类将帮助我们基于事件创建一个*订单*对象——如果有一个*新订单事件*,基于新订单创建一个*订单*…