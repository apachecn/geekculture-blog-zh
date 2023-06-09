# 卡夫卡溪流 vs 阿尔帕卡

> 原文：<https://medium.com/geekculture/kafka-streams-vs-alpakka-8906a90ca286?source=collection_archive---------18----------------------->

![](img/eb95dd7009b3cec0c4d0cc7401a1bb43.png)

Proofpoint 云访问安全代理(CASB)后端数据处理利用 Apache Kafka 进行微服务之间的大多数异步通信。每当我们添加一项新功能或一项新服务时，我们都会问自己:“对于这个用例，什么是正确的流处理技术？”。尽早做出正确的选择对于避免将来的性能问题和代价高昂的重构至关重要。对于我们的大多数数据管道来说，选择归结为 Kafka Streams 还是 Alpakka Kafka。在产品中广泛测试和使用后，我们列出了三个我们认为对决策至关重要的考虑因素。

**处理时间**

在编写基于 Kafka 的应用程序时，需要长时间处理的操作总是一个棘手的问题。虽然 [KIP-62](https://cwiki.apache.org/confluence/display/KAFKA/KIP-62%3A+Allow+consumer+to+send+heartbeats+from+a+background+thread) :“将心跳移动到专用线程”确实在这方面取得了很大的进步，但慢消费者仍然会陷入不必要的再平衡，甚至进入无休止的再平衡状态。这是为什么呢？为了决定消费者是否死亡，协调器查看最后一次成功的心跳时间和最后一次轮询时间。如果使用者在长时间的消息处理操作中没有轮询，它将被认为是死的，使用者组将进行重新平衡。当有问题的慢速消费者完成其长时间的处理操作时，它将尝试轮询新消息，但此时它不再是消费者组的一部分。它将被视为一个新的消费者，另一个再平衡将会发生。现在考虑一下，当一个组有不止一个消费者，而不同的消费者每次都要花很长时间进行轮询时会发生什么。

有两种方法可以解决这个问题:

1.  暂停消费者。这将继续轮询循环，但不会获取数据。
2.  增加 max.poll.interval.ms 或减少 max.poll.records .这带来了两个风险:读取缓慢和当消费者在处理过程中被卡住时的遗漏。

虽然 Kafka streams 不支持暂停消费者，但 Alpakka 依赖于它。

Akka 中的轮询周期暂停 Akka 不能处理的所有分区上的消费者，然后在 Kafka 消费者上调用轮询方法。因此，即使流很忙，不能处理任何实际数据，轮询超时也不会过去。

**内存消耗**

Akka streams buffer 是一个非常棒的功能。它有助于以不同的速率桥接上游和下游，它可以提高性能，并且它是一种实现流背压的机制。然而，没有免费的游乐设施，Akka 缓冲区耗费大量内存。缓冲区并不是唯一需要内存的东西。参与者有一个邮箱，异步动作有一个小的缓冲区来解决性能问题，等等。

另一方面，Kafka Streams 是一个库，它设计了一个更简单的处理模型。它不支持额外的缓冲或背压，因此与 Alpakka 相比不需要额外的内存。

**并行**

Kafka 流中的处理并行性是由运行在该流上的[流处理器线程](https://jaceklaskowski.gitbooks.io/mastering-kafka-streams/content/kafka-streams-internals-StreamThread.html)的数量定义的。因为每个流线程都使用一个消费者，所以它将转化为消费者的数量。因此，Kafka 流并行性将始终受到输入主题中分区数量的限制。

在阿尔帕卡，情况要复杂一些。Akka 流每个图运行一个消费者，这是不可配置的。并行度由 [mapAsync](https://doc.akka.io/docs/akka/current/stream/operators/Source-or-Flow/mapAsync.html) 和 [mapAsyncUnordered](https://doc.akka.io/docs/akka/current/stream/operators/Source-or-Flow/mapAsyncUnordered.html) 动作的数量及其并行度定义。通过向使用者添加一个缓冲区，并添加适量的具有适当并行度的异步操作，您可以获得比输入主题中的分区数量高得多的并发性。理论上，您可以让数千个线程并行处理来自单个分区的消息。

我们不要忘记，Akka 流中的异步操作伴随着一个小的内部缓冲区(这将我们带回到内存消耗部分)。具有大量分区的主题可能需要大量内存。

**真实用例**

Proofpoint CASB 的核心工作是发现云应用程序中的恶意活动和数据泄漏，发出警报，并执行各种补救措施。为此，系统从云提供商那里获取用户活动，并用附加信息丰富它们。这个浓缩过程会进行几次 API 调用，可能会花费一些时间，至少在接近实时的系统中是这样的:在最坏的情况下，可能需要数百毫秒到数十秒。另一方面，大部分时间都在等待 IO 操作完成，而不是处理，因此我们实际上可以在每个 CPU 内核上使用非常高的并行度。对高并发性的需求，加上长处理时间的可能性，使我们将 enricher 组件基于 Alpakka。这样我们就避免了没完没了的重新平衡问题和被输入 Kafka 主题中的分区数量所限制。

CASB 从许多不同的云应用程序中读取数据，这些应用程序都有非常不同的数据模式。我们在数据管道中首先要做的事情之一是将所有输入数据规范化到一个公共模式中。这需要解析 JSON 数据，进行转换，然后将其序列化为输出主题，这主要是一个 CPU 绑定的操作。它不执行 IO，也不受内存限制，因为大多数分配都非常短暂。因此，拥有比 CPU 内核数量更多的并行性实际上是适得其反的。这些考虑结合起来使它成为 Kafka 流的一个非常好的用例。

总之，花些时间了解我们在这里概述的三个核心权衡中的哪一个适用于您的情况，将有助于您构建更健壮、可伸缩、经得起未来考验的数据处理应用程序。