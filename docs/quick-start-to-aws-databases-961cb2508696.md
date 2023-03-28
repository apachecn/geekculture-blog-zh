# AWS 快速入门:数据库

> 原文：<https://medium.com/geekculture/quick-start-to-aws-databases-961cb2508696?source=collection_archive---------5----------------------->

## 第 1 部分:RDS、Aurora 和 DynamoDB 简介

![](img/4863fa43db30722c09a782318076c88a.png)

Photo by [benjamin lehman](https://unsplash.com/@benjaminlehman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

*最后更新时间:2022 年 4 月 20 日*

这是“AWS 快速入门”的另一个版本，旨在帮助个人甚至组织快速轻松地开始使用 AWS 云。它应该对初学者友好并且易于理解，这样就不会让我的观众因为 AWS 的强大功能而感到负担过重。这篇文章关注 AWS 中的数据库，主要是对 DynamoDB、RDS 和 Aurora 的介绍。

大多数应用程序需要一个数据库来存储用户数据、产品等信息。它们是重要的系统，用于存储、管理、更新和检索从办公系统到电子商务网站的所有类型应用程序的数据。

AWS 提供各种数据库服务来满足您的需求。这包括关系数据库，如 MySQL 和 MSSQL，NoSQL 数据库等等。我们将简要介绍 3 种最常见的方法

## 亚马逊 DynamoDB

![](img/2b2c3c17feb06f94d8e0d31adfa5eeb2.png)

Amazon DynamoDB 是由 AWS 提供的完全托管的文档数据库服务，它利用键值并在任何规模下提供小于 10ms 的性能。它支持许多功能，如内置安全性、备份和恢复以及内存缓存。它还支持多区域和多主部署，每天处理超过 10 万亿个请求，每秒钟处理超过 2000 万个请求。

DynamoDB 非常适合移动应用、会话数据存储、游戏应用、物联网和其他需要低延迟读写任意规模数据的应用。完全托管也意味着 DynamoDB 只需要最少的维护，并允许您专注于开发。

据 AWS 称，在工作负载中大量使用 DynamoDB 的公司包括丰田、Lyft 和 Airbnb。

## 亚马逊关系数据库服务(RDS)

![](img/3da52767664c5f8e31b6c4a689f58d32.png)

Amazon RDS 是 AWS 提供的另一个常用的数据库服务。这是一项托管服务，使得在云中设置、操作和扩展关系数据库变得非常快速和简单。RDS 的目标是经济高效且可扩展，同时消除客户的硬件维护、修补和备份等耗时任务。

RDS 允许您根据自己的需求选择不同的数据库实例类型，类似于 EC2。这些产品包括针对内存、性能或 I/O 密集型负载进行优化的实例。它还支持常用的数据库引擎，如 PostgreSQL、MySQL、MariaDB、Oracle、SQL Server 等。

AWS 也意识到了数据库迁移的麻烦，并且还提供了 AWS 数据库迁移服务(DMS)来帮助简化将现有的本地数据库迁移到 RDS 的过程。

据 AWS 称，使用 RDS 的一些受欢迎的公司是 Intuit Mint、国泰航空和三星。

## 亚马逊极光

![](img/0cec81ce21c2ea9589084b6f73c309b2.png)

Amazon Aurora 与 RDS 的相似之处在于它也提供关系数据库，尽管有一些差异和优缺点。与 RDS 不同，Aurora 只兼容 MySQL 和 PostgreSQL。然而，Aurora 比典型的数据库要快得多，也更具成本效益。作为一项全面管理的服务，它还提供了与商业数据库相当的安全性、可用性和可靠性，但成本却很低。

Aurora 处理耗时的任务，如数据库设置、修补、备份、硬件、维护等。它还配备了容错基础架构，通过读取副本、时间点恢复、跨可用性区域复制等功能提供高性能和可用性以及低延迟和冗余。如果您喜欢，Aurora 还提供了无服务器选项。

据 AWS 称，一些利用 Aurora 的热门公司包括三星、DoorDash 和口袋妖怪。