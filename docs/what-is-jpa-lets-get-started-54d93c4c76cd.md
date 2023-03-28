# JPA 是什么？我们开始吧

> 原文：<https://medium.com/geekculture/what-is-jpa-lets-get-started-54d93c4c76cd?source=collection_archive---------28----------------------->

## Java 持久性 API 简介

![](img/59f5d228dc6e43e30594cff438db50fb.png)

Photo by [Terrance Raper](https://unsplash.com/@tkr92?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/bridge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Java 持久性 API 与持久性有关，持久性被广义地定义为允许 Java 对象在生成它们的应用程序进程中存活的任何技术。虽然不是所有的 Java 对象都必须持久化，但是大多数应用程序都必须持久化。JPA 规范允许您指定应该保存哪些对象以及如何在 Java 应用程序中保存它们。

JPA 不是一个工具或框架，而是一个可以被任何工具或框架实现的思想集合。尽管 JPA 的对象关系映射(ORM)范式最初是基于 Hibernate 的，但它随后得到了发展。类似地，虽然 JPA 被设计为与关系/SQL 数据库一起工作，但是某些 JPA 实现已经被修改为与 NoSQL 数据存储一起工作。

## 你应该知道什么？

*   春天
*   Java EE
*   映射 Java 对象
*   CRUD 操作
*   关系数据库管理系统
*   Java 8

## 什么是持久性存储选项？

*   数据库ˌ资料库
*   NoSql
*   文件
*   唱片

## 什么是 ORM(对象关系映射)？

不管它们是如何实现的，每个 JPA 实现都包括某种 ORM 层。要理解 JPA 和 JPA 兼容技术，您必须首先理解 ORM。

对象关系映射是开发人员应该避免手动完成的工作。ORM 层，比如 Hibernate ORM 或 EclipseLink，将任务编码到一个库或框架中。ORM 层是应用程序架构的一部分，负责处理软件对象的转换，以便与关系数据库中的表和列进行通信。Java 中的 ORM 层转换 Java 类和对象，以允许它们在关系数据库中存储和管理。

ORM 就像一座桥，对象映射到表，表映射到对象。

## 如何配置 Java ORM 层？

创建新的 JPA 项目时，必须设置数据存储和 JPA 提供程序。您将设置一个数据存储连接器来连接到您选择的数据库(SQL 或 NoSQL)。您还需要包含并设置 JPA 提供者，它通常是一个类似 Hibernate 或 EclipseLink 的框架。虽然您可以手动设置 JPA，但是许多开发人员更喜欢使用 Spring 的开箱即用支持。

## 有哪些映射方向？

**单向** **连接-** 只有一个实体可以将属性引用给另一个实体。它只有一面，描述了如何执行数据库更新。
**双向连接-** 这种类型的关系既有拥有的一面，也有相反的一面。所以，在这种情况下，每个实体都有一个关系字段，或者引用另一个实体的属性。

## 映射的类型有哪些？

*   **一对一**—[@ One toone](http://twitter.com/OneToOne)注解代表了这种关系。每个实体的实例都链接到另一个实体的单个实例。
*   **一对多** —注解 [@OneToMany](http://twitter.com/OneToMany) 代表了这种关系。在这种关系中，一个实体的实例可以连接到另一个实体的多个实例。
*   **多对一**—[@ Many toone](http://twitter.com/ManyToOne)注释定义了这种映射。在此连接下，一个实体的多个实例可以连接到另一个实体的单个实例。
*   **多对多**—[@ ManyToMany](http://twitter.com/ManyToMany)注释代表了这种关系。在此上下文中，一个实体的多个实例可以连接到另一个实体的多个实例。在这个映射中，任何一方都可能是欠方。

## ORM 有什么好处？

*   生产力
*   应用设计
*   代码的清晰分离
*   维护

我希望你对 JPA 有了基本的了解，我也希望给你更多关于 JPA 的文章，因为还有很多东西你需要知道。那么，让我们来看看另一篇文章。快乐编码^_^