# 卡夫卡入门

> 原文：<https://medium.com/geekculture/getting-started-with-kafka-c0e94ae4d81b?source=collection_archive---------8----------------------->

![](img/8d09775b9bcbde9bf68b6398dad0650f.png)

Credits : [David](https://davidxiang.com)

A pache Kafka 是一个开源分布式事件流平台，被数千家公司用于高性能数据管道、流分析、数据集成和关键任务应用。

核心能力:

*   高吞吐量
    使用低延迟的机器集群以网络有限的吞吐量传递消息
*   可扩展的
    将生产集群扩展到多达一千个代理、每天数万亿条消息、数 Pb 的数据、数十万个分区。
*   永久存储
    将数据流安全地存储在一个分布式、持久、容错的集群中。
*   高可用性
    跨可用性区域高效地扩展集群，或者跨地理区域连接独立的集群。
*   关键任务
    支持关键任务用例，保证有序、零消息丢失和高效的一次性处理。
*   受到成千上万 ORGS 人的信任
    成千上万的组织使用 Kafka，从互联网巨头到汽车制造商到证券交易所。
*   连接任何东西
    Kafka 的开箱即用连接界面集成了数百个事件源和事件接收器，包括 Postgres、JMS、Elasticsearch、AWS S3 等等。

现在我们开始进入编码部分！
第一步:获取 KAFKA
[下载](https://www.apache.org/dyn/closer.cgi?path=/kafka/3.0.0/kafka_2.13-3.0.0.tgz)最新版 KAFKA 并解压:

> `$ tar -xzf kafka_2.13-3.0.0.tgz
> $ cd kafka_2.13-3.0.0`

步骤 2:启动 KAFKA 环境
运行以下命令，以正确的顺序启动所有服务:

> `# Start the ZooKeeper service
> # Note: Soon, ZooKeeper will no longer be required by Apache Kafka.
> $ bin/zookeeper-server-start.sh config/zookeeper.properties`

在另一次终端运行中

> `# Start the Kafka broker service
> $ bin/kafka-server-start.sh config/server.properties`

步骤 3:创建一个主题来存储你的事件。打开另一个终端会话并运行:

> `$ bin/kafka-topics.sh --create --partitions 1 --replication-factor 1 --topic quickstart-events --bootstrap-server localhost:9092`

Kafka 的所有命令行工具都有附加选项:不带任何参数运行`kafka-topics.sh`命令来显示用法信息。

> `$ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
> Topic:quickstart-events PartitionCount:1 ReplicationFactor:1 Configs:
> Topic: quickstart-events Partition: 0 Leader: 0 Replicas: 0 Isr: 0`

第四步:将一些事件写入主题
Kafka 客户端通过网络与 Kafka 代理通信，以写入(或读取)事件。一旦接收到事件，代理就会以持久和容错的方式存储事件，只要您需要，甚至是永远。

默认情况下，您输入的每一行都会导致一个单独的事件被写入主题。

> `$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
> This is my first event
> This is my second event`

步骤 5:读取事件
打开另一个终端会话并运行控制台消费者客户端来读取您刚刚创建的事件:

> `$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
> This is my first event
> This is my second event`

您可以随时用`Ctrl-C`停止消费客户端。因为事件被持久地保存在卡夫卡中，它们可以被你想要的任何次数和任何消费者阅读。您可以通过打开另一个终端会话并再次运行前面的命令来轻松验证这一点。

第 6 步:使用 KAFKA CONNECT 将您的数据作为事件流导入/导出
Kafka Connect 允许您从外部系统向 KAFKA 连续摄取数据，反之亦然。因此，将现有系统与 Kafka 整合起来非常容易。为了使这个过程更容易，有数百个这样的连接器随时可用。

步骤 7:用 KAFKA STREAMS 处理您的事件
一旦您的数据作为事件存储在 KAFKA 中，您就可以用 KAFKA STREAMS client library for Java/Scala 来处理数据。它允许您实现任务关键的实时应用程序和微服务，其中输入和/或输出数据存储在 Kafka 主题中。Kafka Streams 将在客户端编写和部署标准 Java 和 Scala 应用程序的简单性与 Kafka 服务器端集群技术的优势相结合，使这些应用程序具有高度可伸缩性、弹性、容错性和分布式。该库支持一次性处理、有状态操作和聚合、窗口、连接、基于事件时间的处理等等。

第八步:终止 Kafka 环境
现在你已经到了快速入门的末尾，可以随意拆除 KAFKA 环境了——或者继续玩。

1.  使用`Ctrl-C`停止生产者和消费者客户端，如果您还没有这样做的话。
2.  用`Ctrl-C`阻止卡夫卡经纪人。
3.  最后，用`Ctrl-C`停止 ZooKeeper 服务器。

如果您还想删除本地 Kafka 环境的任何数据，包括您在此过程中创建的任何事件，请运行命令:

> `$ rm -rf /tmp/kafka-logs /tmp/zookeeper`

更多信息请查看官方文档！干杯！