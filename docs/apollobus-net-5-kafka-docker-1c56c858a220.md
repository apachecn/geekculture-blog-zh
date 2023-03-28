# 阿波罗布斯+。Net 5 +卡夫卡+ Docker

> 原文：<https://medium.com/geekculture/apollobus-net-5-kafka-docker-1c56c858a220?source=collection_archive---------10----------------------->

我将向您展示如何使用 ApolloBus 通过 Kafka 和 Docker 发布和订阅事件/消息。

# 阿波罗布斯

ApolloBus 是一个集成了 RabbitMQ、Kafka 和 ServiceBus 的 EventBus。ApolloBus 允许您抽象消息的发布和订阅。

我想使用消息传递在两个服务之间进行通信。使用 ApolloBus，我需要为 Kafka 添加配置。

# 卡夫卡

> 卡夫卡是一个分布式系统…