# 反应器 Kafka 驱动器

> 原文：<https://medium.com/geekculture/reactor-kafka-driver-3d400caed15?source=collection_archive---------2----------------------->

Reactor Kafka 是 Kafka 的一个反应式 API，基于 Reactor 和 Apache Kafka 生产者/消费者 API。Reactor Kafka API 支持将消息发布到 Kafka，并使用具有非阻塞背压和非常低的开销的功能性 API 从 Kafka 消费消息。

在本文中，我们将深入讨论反应器 Kafka API。我们关注消费、生产场景、流水线、事务发送和线程模型。

# 反应器 Kafka 接收器

## KafkaReceiver 接口