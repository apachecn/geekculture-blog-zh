# 用 JUnit 5 对单个类进行单元测试的初学者指南

> 原文：<https://medium.com/geekculture/beginners-guide-to-mockito-unit-testing-on-single-classes-with-junit-5-b9346e1cd4c7?source=collection_archive---------2----------------------->

![](img/9fd2a5f2e393b95640bcac5aab207697.png)

# 方案

编写单元测试是确保代码按预期工作的一种重要方式。在某些情况下，我们想要为其编写测试的类依赖于其他类。在这样的场景中，我们可以通过使用 Mockito 库来帮助我们模仿依赖类，并且只关注类，从而避免构建那些依赖类…