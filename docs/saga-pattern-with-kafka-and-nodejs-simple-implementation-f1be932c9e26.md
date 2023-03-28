# Kafka 和 NodeJS 的 Saga 模式:简单的实现

> 原文：<https://medium.com/geekculture/saga-pattern-with-kafka-and-nodejs-simple-implementation-f1be932c9e26?source=collection_archive---------0----------------------->

每当您决定将应用程序从单片架构重建为微服务架构时，您都会遇到一个主要困难。这个困难是为涉及几个微服务的操作实现一个分布式事务。

希望有几种模式可以应用。至少有两种方法可用: ***两阶段提交*** 或 ***传奇*** 。这里我们只深入考虑家族模式。

让我们考虑一个典型的真实例子，我们需要实现…