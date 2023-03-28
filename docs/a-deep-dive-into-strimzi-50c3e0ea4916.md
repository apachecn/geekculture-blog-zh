# 对 Strimzi 的深入研究

> 原文：<https://medium.com/geekculture/a-deep-dive-into-strimzi-50c3e0ea4916?source=collection_archive---------1----------------------->

[Strimzi](https://github.com/strimzi/strimzi-kafka-operator) 是一个开源项目，为在 Kubernetes 上运行 Apache Kafka 提供容器映像和 Kubernetes 操作符。

在这篇文章中，我们仔细看看 strimzi 是如何在幕后工作的。

## 内容:

*   垂直 x
*   Kubernetes 告密者背景
*   Strimzi 算子
*   用户操作员
*   聚类算子
*   主题运算符
*   实体运算符

# 0.垂直 x