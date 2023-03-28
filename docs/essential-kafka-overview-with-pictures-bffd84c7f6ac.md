# 带有图片的卡夫卡概述

> 原文：<https://medium.com/geekculture/essential-kafka-overview-with-pictures-bffd84c7f6ac?source=collection_archive---------5----------------------->

现在*事件驱动的*架构在现代开发中越来越流行。一个原因是它允许我们构建一个解耦的系统，这是微服务领域的一个重要特性。

1.  **卡夫卡服务器建筑师**

Kafka 是一个健壮的发布/订阅系统，由 LinkedIn 开发，用 Java 和 Scala 编写。Kafka 可能包含一组被称为 ***代理*** 的 ***Kafka 节点*** (服务器)，它创建了一个可横向扩展的 ***集群*** 。Kafka 架构还包括一个轻量级的 Apache Zookeeper 服务器，它负责存储…