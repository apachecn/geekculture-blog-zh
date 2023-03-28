# Apollo bus+. net 5+service bus Azure

> 原文：<https://medium.com/geekculture/apollobus-net5-servicebus-azure-d261da5839f8?source=collection_archive---------11----------------------->

在本文中，我将演示如何将 ApolloBus 与 ServiceBus 一起使用。

# 阿波罗布斯

ApolloBus 是一个集成了 RabbitMQ、Kafka 和 ServiceBus 的 EventBus。ApolloBus 允许您抽象消息的发布和订阅。

我想使用消息传递在两个服务之间进行通信。使用 ApolloBus，我需要为队列或主题和订阅添加一个配置。

# Microsoft Azure 服务总线