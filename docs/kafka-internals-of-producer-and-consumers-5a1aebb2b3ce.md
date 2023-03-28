# 卡夫卡——生产者与消费者的内在

> 原文：<https://medium.com/geekculture/kafka-internals-of-producer-and-consumers-5a1aebb2b3ce?source=collection_archive---------0----------------------->

![](img/664e1ca90e61769342806c9f36aaa560.png)

Photo by [Tom Pottiger](https://unsplash.com/@tompottiger?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/producer-consumer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

将数据移入和移出 Kafka

在我上一篇博客中，我讨论了卡夫卡的基本架构。在这个博客中，我们会看到更多关于生产者和消费者的细节。

# 生产者

将数据发布到 Kafka 主题的应用程序被称为生产者。应用程序集成了一个 Kafka 客户端库来写入 Kafka。编写过程从创建一个 ProducerRecird 开始。

![](img/a2fdcfe2da883c2f827f4d31066eeb3e.png)

## Kafka 生产商的组件/流程

*   **拦截器** —能够在发送之前改变记录的拦截器，例如[Claim-check-interceptor](https://github.com/irori-ab/claim-check-interceptors)。
*   **生产者元数据** —管理生产者所需的元数据:集群中的主题和分区、作为分区领导者的代理节点等。
*   **序列化器** —将对象转换为字节数组的键/值序列化器。
*   **分区器** —计算给定记录的分区。如果在 ProducerRecord 中指定了分区，那么分区器将返回相同的结果，否则，它将根据分区策略(循环、散列键或自定义分区)为消息键选择一个分区。`org.apache.kafka.clients.producer.internals.DefaultPartitioner, org.apache.kafka.clients.producer.RoundRobinPartitioner, org.apache.kafka.clients.producer.UniformStickyPartitioner, org.apache.kafka.clients.producer.Partitioner (Inteface)`

![](img/e566a90ac472ea99127942a7a393ddb3.png)

*   **记录累积器** —累积记录，并通过主题分区将其分组为批次。一批未发送的记录保存在缓冲存储器中。一个单独的 I/O 线程(sender 线程)负责将这些批次的记录作为请求发送给 Kafka 代理。
*   **事务管理器** —管理事务并维护必要的状态以确保幂等生产。
*   **频道选择器** —创建一个网络客户端，与代理建立通信。

## 生产者确认设置

Kafka 生产者只将数据写入分区的当前主代理。如果我们希望消息在被认为是成功写入之前必须被写入最小数量的副本，我们需要设置`acks`,这是在消息被认为是成功写入之前需要确认收到消息的代理的数量。

![](img/77e4869472928bd70036c22ed3431efc.png)

Acks = all and min.insync.replicas = 3

注意:当`acks=all`有`replication.factor=N`和`min.insync.replicas=M`时，我们可以容忍`N-M`经纪人因为话题可用性的原因而倒下。

## 卡夫卡制片人重试

![](img/39dfe4bbe63d8f2929f32f953b819824.png)

> retries
> delivery . time out . ms
> retry . back off . ms
> max . in-flight . requests . per . connection

## 幂等卡夫卡生产者

重试发送失败的消息有重复的小风险。如果数据被复制到 ISR，但确认没有到达生产者，因此重试，则可能发生这种情况。为了避免这种情况，卡夫卡使用了不断增加的 PID 序列。Kafka 总是采用成功编写的最大 PID-序列号组合。当收到较低的序列号时，它将被丢弃。

> enable . idempotence = true
> ack = all

## 卡夫卡信息压缩

> compression.type
> none、gzip、lz4、snappy 和 zstd

![](img/df79c7537338f9b090a69e0538290bf5.png)

如果我们使用生产者级压缩，那么我们应该将代理级设置设为`compression.type=producer`。如果生产者级压缩和代理级压缩不匹配，代理将解压缩并再次压缩。

## Kafka 生产者配料

> linger.ms —发送批次前等待的时间
> batch.size —批次中包含的最大字节数

![](img/67a88fc8550a573be6dddedabe46655b.png)

## 删除消息

可以通过发布一条新消息来删除 Kafka 主题中的消息，这条新消息的关键字是我们想要删除的消息的关键字`NULL`。Kafka 将找到那些具有空值的键，并删除该消息。我们需要设置`delete.retention.ms`来控制何时删除消息。

# 经纪人

![](img/19c76e0cfadd77be2f19803696400c21.png)

*   生产者记录位于套接字接收缓冲区。其中一个网络线程获取消息，并将其传递给共享请求队列。
*   该记录由 I/O 线程拾取。它验证数据的 CRC。然后，该记录被写入提交日志。
*   I/O 线程将响应逻辑交给炼狱映射(管理延迟操作的代理。).此映射等待其他代理确认写入(ISR)。这个映射是用 ConcurrentHashMap 和 ConcurrentLinkedQueue 实现的。
*   消息复制后，一个响应将被放入响应队列。
*   网络线程从队列中取出响应，并将其放入套接字发送缓冲区。

> 代理需要内存 30 Gb+(用于 pagecache)
> 代理需要多核(多线程 app)
> 代理需要 1Gbps +网络(用于通信)
> 代理需要可靠的磁盘( *st1* EBS)

> *考虑到上述情况，建议 AWS 上的 EC2 为 r5.xlarge* 带 EBS。

当我们启动 Kafka 集群时，代理将首先创建一个与 zookeeper 的会话，他们将尝试在 zookeeper 内创建一个临时节点**"*/控制器* "** 。能够成功创建“***/控制器*** ”节点的代理将成为控制器。其余的代理将在这个“***/控制器*** ”节点上创建一个监视。

如果控制器关闭或与 zookeeper 的会话丢失，则该 znode 将被删除，其余的代理将被通知，并将再次选举新的控制器。

```
zookeeper-shell.sh localhost:2181
ls /
ls /brokers/ids
get /controller
```

## 主计长的责任

> 代理活跃性
> 领导者选举
> 更新 ISR

# 消费者

从 Kafka 主题中读取数据的应用程序被称为消费者。应用程序集成了一个 Kafka 客户端库，可以从 Apache Kafka 中读取数据。消费者从一个或多个分区读取数据，每个分区内的顺序保持不变。卡夫卡的消费者实行一种“拉动模式”。这意味着消费者向代理发送获取请求以获取数据。

![](img/ee2b358ee90991a9a838f1b1bf0d4ed7.png)

从上图中，我们看到了卡夫卡《消费者》中的以下成分。

*   **协调者** —管理组成员、补偿
*   **元数据** —管理消费者所需的元数据:集群中的主题和分区、作为分区领导者的代理节点等。
*   **网络客户端** —处理与代理的连接/请求
*   **提取器** —从经纪人处提取批量记录。
*   **解串器** —将字节数组转换为对象的键/值解串器。
*   **拦截器** —可能改变记录的拦截器

## 面向消费者的交付语义

> enable.auto.commit=true
> 自动提交. interval.ms

*   **最多一次:**收到消息后立即提交补偿。如果出现错误，消息可能会丢失。
*   **至少一次:**消息处理后提交偏移量。可能会导致多次读取。确保消息处理是等幂的。
*   **恰好一次:**只在卡夫卡→卡夫卡随交易流动。

## 其他配置:

> fetch . min . bytes
> fetch . max . wait . ms

增加 fetch.min.bytes 和时间将导致吞吐量增加，减少它将导致更好的延迟。

带有 PEM 的 SSL 侦听器:

```
# content of client.properties
bootstrap.servers=kafka.server:9092
security.protocol=SSL
ssl.truststore.type=PEM
ssl.truststore.location=truststore.pemssl.keystore.type=PEM
ssl.keystore.location=keystore.pem
ssl.key.password=hello

# encrypt your private key in PKCS8
openssl pkcs8 -topk8 -in privateKey.key -out encryptedPrivateKey.p9

# Copy the content of your encrypted private key and put into the keystore.pem
# follow the exact order

-----BEGIN ENCRYPTED PRIVATE KEY-----
xxxx
-----END ENCRYPTED PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
xxxx
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
xxxx
-----END CERTIFICATE-----
```

快乐学习！！