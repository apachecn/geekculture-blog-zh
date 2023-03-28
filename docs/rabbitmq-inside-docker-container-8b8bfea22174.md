# docker 容器内的 RabbitMQ

> 原文：<https://medium.com/geekculture/rabbitmq-inside-docker-container-8b8bfea22174?source=collection_archive---------2----------------------->

在之前的[故事中，我描述了如何在 Windows 上安装 RabbitMQ。](https://alex-ber.medium.com/installing-rabbitmq-on-windows-4411f5114a84)

在这里，我将描述如何通过管理 UI 获得 RabbitMQ。

最简单的方法就是使用 docker image*rabbit MQ:3 . 8 . 9-管理-alpine* (我更喜欢基于 Alpine Linux 的分发)*。*Alpine Linux 的源代码可以在这里找到[https://github . com/docker-library/rabbit MQ/blob/888638927482 f 86 af 6e 88 bebb 67423926 CB 1112 f/3.8/Alpine/management/docker file](https://github.com/docker-library/rabbitmq/blob/888638927482f86af6e88bebb67423926cb1112f/3.8/alpine/management/Dockerfile)

它不能将消息从一个队列移动到另一个队列(插件*rabbit MQ _ slope*和*rabbit MQ _ slope _ management*没有安装)。下面的代码解决了这个问题:

我只改变了第 4 行:

```
RUN rabbitmq-plugins enable --offline rabbitmq_management rabbitmq_shovel rabbitmq_shovel_management
```

我加了`rabbitmq_shovel rabbitmq_shovel_management`。

假设您已经构建了这个名为`rabbitmq-i`的 docker 映像。

现在，您的代码通常会在另一个 docker 文件中运行。在两个 docker 容器之间建立网络连接的最佳实践是创建网络。详见[https://www . middleware inventory . com/blog/docker-connect-containers-together/](https://www.middlewareinventory.com/blog/docker-connect-containers-together/)。

```
docker network create rabbitnet
```

现在，通过以下方式启动基于`rabbitmq-i` docker 映像的 docker 容器:

```
docker run -d --hostname rabbitmq --name rabbitmq -p 15672:15672 -p 5672:5672 --network rabbitnet  rabbitmq-i
```

`-d` —以分离模式启动(在后台，不将控制台连接到进程的标准输入、标准输出和标准错误)。

`--hostname` —容器的主机名。它在 RabbitMQ 内部使用。例如，作为 RabbitMQ 集群名称的一部分

![](img/186cd2678b539e683e2868381ec53e25.png)

From the management UI

`--name` —码头集装箱的名称。它将用于从另一个 docker 容器与这个 docker 容器进行通信。最好和`hostname`一样。

`-p 15672:15672 -p 5672:5672` — `5672`用于连接 RabbitMQ，端口`15672`用于管理 UI。我使用的是普通 TCP，所以我公开了端口`15672`。如果你想使用 TLS，你应该公开端口`15671`。引用自文档:

> *T*T28*LS 同行验证:你说你是谁？*
> 
> *正如在* [*证书和密钥*](https://www.rabbitmq.com/ssl.html#certificates-and-keys) *一节中提到的，TLS 有两个主要目的:加密连接流量和提供一种方法来验证对等体是否可信(例如，由可信的证书颁发机构签名)以缓解* [*中间人攻击*](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) *，这是一类攻击者冒充合法可信对等体(通常是服务器)的攻击。本节将重点讨论后者。*

[https://www.rabbitmq.com/ssl.html](https://www.rabbitmq.com/ssl.html)

`--network rabbitnet` —这是定义的关键部分，我们将`rabbitmq-i`连接到内部网络(名称为`rabbitnet`)。

# 代码示例

现在，假设您有另一个安装了`pika==1.1.0`的 docker 容器。那么您可以使用以下阻塞消息生成器:

当你运行这个 docker 容器**时，你应该添加** `**--network rabbitnet**` **到** `**docker run**`指令，以便能够与`rabbitmq` docker 容器通信(也就是坐在`rabbitnet`上，见上文)。

让我们看一下代码:

*在第 8 行——教程中的`URLParameters`常用*。`URLParameters` 和`ConnectionParameters`都扩展了`Parameters`类。`BlockingConnection` 期望`Parameters`类被传递给它的`__init__`方法，所以你可以同时使用它们。我使用所有默认值，除了`host`。我用`rabbitmq`做主机。因为这个 docker 容器是基于名为`rabbitmq`的`rabbitmq-i`映像通过`**rabbitnet**`连接到 docker 容器的，所以我们的 TCP 调用将到达带有 RabbitMQ 的 docker 容器。

> *注意:同样的代码也适用于安装在 Windows* *机器上的* [*RabbitMQ。您应该只修改 C:\ Windows \ System32 \ drivers \ etc \ hosts 文件。添加以下一行:*](https://alex-ber.medium.com/installing-rabbitmq-on-windows-4411f5114a84)
> 
> 127.0.0.1 兔子质量
> 
> *现在，您对“rabbitmq”主机名的调用将到达您的本地主机(您安装了 RabbitMQ 的地方)。*

*在第 10 行* —我们用上面的连接参数创建最简单的`BlockingConnection`。请参见[比较消息发布与阻塞连接和选择连接](https://pika.readthedocs.io/en/stable/examples/comparing_publishing_sync_async.html)、[使用回调传递方式连接到 RabbitMQ、](https://pika.readthedocs.io/en/stable/examples/connecting_async.html)[异步发布者示例](https://pika.readthedocs.io/en/stable/examples/asynchronous_publisher_example.html)了解替代方法(使用`Pika`)。我们使用上下文管理器在最后自动关闭连接。

*在第 12 行*——我们从上面的`connection` 得到`channel`。

*第 15 行(未使用)——*我们可以通过编程来创建队列。就我个人而言，我更喜欢在管理 UI 中完成，但是你也可以在代码中完成。引用自 docstring:

```
*Declare queue, create if needed. This method creates or checks a
queue. When creating a new queue the client can specify various
properties that control the durability of the queue and its contents, and the level of sharing for the queue.

Use an empty string as the queue name for the broker to auto-generate one. Retrieve this auto-generated queue name from the returned `spec.Queue.DeclareOk` method frame.**:param str queue: The queue name; if empty string, the broker will
    create a unique queue name
:param bool passive: Only check to see if the queue exists and raise
  `ChannelClosed` if it doesn't
:param bool durable: Survive reboots of the broker
:param bool exclusive: Only allow access by the current connection
:param bool auto_delete: Delete after consumer cancels or disconnects
:param dict arguments: Custom key/value arguments for the queue**:returns: Method frame from the Queue.Declare-ok response**:rtype: `pika.frame.Method` having `method` attribute of type
    `spec.Queue.DeclareOk`*
```

[https://github . com/pika/pika/blob/master/pika/adapters/blocking _ connection . py](https://github.com/pika/pika/blob/master/pika/adapters/blocking_connection.py)

*在第 18 行* —我们正在启动适当的机制来确保*消息传递到 RabbitMQ。*更多详情见[https://www.rabbitmq.com/confirms.html](https://www.rabbitmq.com/confirms.html)

*在第 20–25 行—* 我们实际上是在向 RabbitMQ 发送消息。`basic_publish`方法将消息发布到特定的交换机。消息将被路由到 exchange 配置定义的队列，并在提交事务(如果有)时分发给任何活动的使用者。

空的*交换*名称意味着应该使用默认交换。

*routing_key* —绑定的路由键。对于默认交换，可以提供*队列*名称，因此消息将被发送到适当的队列。在这里，我们发送到`local`队列。

*正文* —消息正文。

*属性(未使用)—消息的基本属性*。

*   *delivery_mode=1* 表示*瞬态交货模式。*这意味着消息将不会存储在磁盘上，并将在代理重新启动后消失。
*   *delivery_mode=0* 表示持续交付模式。您还应该将队列声明为*持久的*(参见上面的`queue_declare`；您也可以在管理 UI 中执行此操作)。在这种情况下，消息将在代理重新启动后存储在磁盘上。

> [*坚持是如何工作的*](https://www.rabbitmq.com/persistence-conf.html#how-it-works)
> 
> *首先，介绍一些背景知识:持久消息和瞬时消息都可以写入磁盘。持久性消息将在到达队列后立即被写入磁盘，而暂时性消息将只被写入磁盘，以便在内存紧张时可以从内存中将其逐出。持久性消息也尽可能地保存在内存中，只有在内存压力下才会从内存中清除。“持久层”是指用于将两种类型的消息存储到磁盘的机制。*

[https://www.rabbitmq.com/persistence-conf.html](https://www.rabbitmq.com/persistence-conf.html)

*强制—* 此标志告知服务器在消息无法路由到队列时如何反应。如果设置了此标志，服务器将使用返回方法返回不可路由的消息。如果该标志为零，服务器会自动丢弃该消息。

参见[https://www . rabbit MQ . com/amqp-0-9-1-reference . html # basic . publish](https://www.rabbitmq.com/amqp-0-9-1-reference.html#basic.publish)

基本上，这段代码使用 **push** 模型:RabbitMQ 在这段代码上“推送”消息——来自`local`队列的每个消息都将在`on_message()`回调中处理。

*注意*:在 PyCharm `Ctrl+C`对我不起作用的情况下，我无法发送`SIGINT` 信号来中断`channel.start_consuming()`方法。终止进程会在 RabbitMQ 中打开连接/通道。请务必手动关闭它。从 CLI 运行这段代码更好。

详见[https://pika . readthe docs . io/en/stable/examples/blocking _ consume . html](https://pika.readthedocs.io/en/stable/examples/blocking_consume.html)。

基本上，这段代码使用了 **pull** 模型:它从 RabbitMQ“拉”出消息。这段代码可以完全控制何时停止处理消息。它还能够知道队列何时为空。

*注:*我用的是`Twisted==20.3.0`。您需要安装一些额外的包来使这些代码工作。

参见[https://pika . readthe docs . io/en/stable/examples/blocking _ consumer _ generator . html](https://pika.readthedocs.io/en/stable/examples/blocking_consumer_generator.html)

我不会看完每一行，只看有趣的一行。

在第 74–76 行，我们将至少 2 条消息放入队列中(以便它不为空)。

在第 12–13 行，我们有两个标志— *完成*和*完成 _ 延期。*该变量用于不同线程之间的通信(细节将在下面提供)。

*注意:* Python 将*所有的对象*存储在主内存的一个堆上。所以在 Python 中不需要使用`volatile`。详见[https://stackoverflow.com/a/53780395/1137529](https://stackoverflow.com/a/53780395/1137529)。

*已完成—* 队列已完成填充—生产者将所有项目放入队列。

*已完成 _ 延迟—* 队列已完成处理—队列中的所有项目都已消耗。

在第 51–52 行

`#installSignalHandlers
Thread(target=reactor.run, args=(False,)).start()`

我们正在启动专用线程中的扭曲反应堆。

一般来说，在 Twisted 应用程序中，反应器应该在主线程中启动。我们没有在主线程中这样做，我们不能安装信号处理器，所以我们通过了`False`。这不是一种标准的方法，因为我不想在这里编写 Twisted 应用程序，但是我想使用 Twisted event-loop 来使用队列中的项目。

在第 60 行:

`threads.deferToThread(defer_pika_queue_consume, channel)`

这段代码在运行 Twisted event-loop 的专用线程中运行`defer_pika_queue_consume(channel)`方法。

在第 62–63 行(`main()`函数)，我们有

```
global finished
finished = True
```

这里我们模拟生产者已经完成填充队列。我们正在将此消息发送给`on_message`回调。现在，如果队列是空的，他应该停止队列消费。默认情况下，如果队列为空，它将等待新消息到达。

在第 65–66 行(`main()`函数)中，我们有

```
while not finished_defered:
    times.sleep(20)
```

`main()`函数将等待，直到`on_message()`回调将向我们发送信号，表明队列中的所有项目都已消耗，我们可以关闭到队列的连接/通道。

*注:*

1.  这里我们使用了`CountDownLatch.`的自旋锁实现
2.  这个实现不是基于互斥的(不是基于`Lock` / `RLock`)。详见[https://stackoverflow.com/q/10236947/1137529](https://stackoverflow.com/q/10236947/1137529)。
3.  我们不能在这里使用`connection.sleep()`，因为我们在不同的线程中有反应器事件循环。

在`on_message()`回调中:

在第 32 行

```
for method_frame, properties, body in channel.consume(“local”, inactivity_timeout=10.0)
```

我们传递 *inactivity_timeout* 是为了在超时后退出对`consume()`方法的调用，以便检查`finished`标志。如果是`True`并且队列是空的，我们应该完成消费队列中的物品。

在第 39–44 行—在我们将`finished`标记为`True` 之后，我们使用`channel.get_waiting_message_count()`调用来检查我们是否已经处理了队列中的最后一个项目。这避免了排队等候。以下代码也是正确的:

但是这不是最佳的:这里我们必须至少有一个阻塞调用来打破循环。

在所提供的例子中，如果在最后一个项目被处理之前收到了`finished`消息，我们将永远无法阻止和中断循环。