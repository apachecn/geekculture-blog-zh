# 兔子铁锹

> 原文：<https://medium.com/geekculture/rabbitmq-shovel-8e6ccdf309b?source=collection_archive---------10----------------------->

> "这样一把铲子，不使用似乎是一种浪费."
> ― **丹尼尔·克劳斯，** [**罗特**](https://www.goodreads.com/work/quotes/13440919)

![](img/7c081df44144c4137b29c0b72bd6a445.png)

Photo by [Andrey Metelev](https://unsplash.com/@metelevan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在消息代理 [*RabbitMQ*](https://www.rabbitmq.com/) 中，典型的设置是部署驻留在一个盒子中的单个 RMQ 实例。RMQ 的工作是处理传入的消息，并确保这些消息被转发到正确的目的地。

发布者将消息发送到作为消息路由代理的交换机。交换基于路由关键字将消息路由到不同的队列。每个队列都有附加的消费者，他们将消费和处理消息。

![](img/68a307387df3ab8468595ff8c636385e.png)

RabbitMQ Broker

现在，如果我们需要一个基于队列的系统，在这个系统中，我们在一个代理中有多个队列，并且每个 RMQ 实例可以相互发送和接收消息，那该怎么办呢？第二个用例可能是在不同的 RMQ 代理中将消息从一个队列卸载到另一个队列。 ***这就是铲子发挥作用的地方。***

R️abbitMQ 铁铲不断地将信息从一个来源移动到一个目的地。源和目标可以位于具有不同虚拟主机的同一集群中，也可以位于不同的地理和管理域中。它可以在同一个代理的队列和交换之间移动消息，也可以充当两个不同代理之间的桥梁。

铲子可以用来平衡队列的负载，或者当我们需要从一个 RabbitMQ 代理中取出消息并插入到另一个代理中时。本质上，铲**将**连接到源和目的地代理，**从队列中消费**消息，**将每个消息重新发布**到目的地代理。

![](img/22ad417a518ce0f6a455b583157600f2.png)

RabbitMQ Shovel

# 为什么我们需要铲子

*   **松耦合** 铲子可以在不同的 RMQ 集群之间移动消息，这些集群可能有不同的用户、RMQ 版本、vhosts 和消息协议(AMQP 0.9.1 和 AMQP 1.0)。
*   **万友善** 铲罩下采用客户端连接。它通过执行[消费者确认](https://www.rabbitmq.com/confirms.html)来容忍间歇性连接而不丢失消息。
*   **可配置参数** 铲子是高度可定制的，可以配置成执行任意数量的显式方法。

# 铲子的类型

有两种类型的铲-静态铲和动态铲。

**静态铲**在配置文件中定义，需要节点重启才能应用任何更新。这在本质上更为普遍，因为队列、交换和绑定需要在启动时手动声明。如果集群中存在多个 RMQ 节点，那么应该在配置文件中为所有节点定义一个静态铲。每一铲只会在一个节点上开始，如果这个节点出故障，它会移动到另一个健康的节点。

**动态铲**在运行时参数中定义。它们可以随时创建和删除，不需要重新启动节点。这些更加自动化友好，因为铲子使用的队列和交换是自动声明的。在动态铲中，我们不需要为所有节点手动定义它。这是自动完成的，每个铲将只在一个节点上启动，并在需要时故障转移到另一个节点。

# 架起铲子

## 启用铲子插件

安装铲子是一帆风顺的。这可以通过使用 [RMQ 命令行界面](https://www.rabbitmq.com/cli.html)来完成。

```
rabbitmq-plugins enable rabbitmq_shovel 
rabbitmq-plugins enable rabbitmq_shovel_management
```

## 铲子测绘

铲映射可以从 RMQ 管理仪表板创建。转到*管理*部分，点击*铲管理*。

[RMQ URI](https://www.rabbitmq.com/uri-spec.html) 的语法是`amqp://username:password@host:port/vhost`

![](img/966b28ec00f891073e034d1b44c24344.png)

RabbitMQ — Shovel Management

**铲参数:**

1.  虚拟主机:从下拉列表中选择 vhost。
2.  铲子的名称。
3.  来源:输入 RMQ URI 和来源队列/交换。
4.  目的地:目的地实例和队列/交换的 RMQ·URI。

一旦所有细节就绪，点击*添加铲子*按钮。

铲的**状态**可以从*管理* > *铲状态*标签中看到。状态值可以是:

*   *启动*:铲子正在启动并创建资源，尝试连接端点。
*   *运行*:铲子成功连接到两个端点，从源消费，重新发布到目的地。
*   *终止*:铲已经终止或出现异常。原因可以在日志中看到。