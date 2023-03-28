# 跳靴:自动装配第 1 部分

> 原文：<https://medium.com/geekculture/springboot-automatic-assembly-part-1-e5415ef44b0b?source=collection_archive---------44----------------------->

在前一篇文章的[中，我们探讨了 bean 类如何在 bean 工厂中注册，我们详细研究了负责扫描和注册各种组件的核心 bean 处理器。](https://deepkondah.medium.com/springboot-registering-bean-definitions-4f362b305218)

本文的目的是看一看 beans 初始化，在介绍之前，让我们先介绍一些概念。

## 控制反转(IoC)

控制反转是一种描述外部实体(容器)的模式，负责在创建时通过注入…