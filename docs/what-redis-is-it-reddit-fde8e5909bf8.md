# 雷迪斯？为什么！

> 原文：<https://medium.com/geekculture/what-redis-is-it-reddit-fde8e5909bf8?source=collection_archive---------1----------------------->

## 酷派科技系列

## 这篇文章解释了 redis.io 上 Redis 描述中提到的每一件事，这意味着我将清楚而浅显地理解这一切，只是一个很好的概述，并希望给你开始使用它的动力。

![](img/761772f831bd7ef7bcdcd33fb4bf8f66.png)

[Thanks Wikipedia for this image and so much more!](https://en.wikipedia.org/wiki/Redis)

通常要理解一件事，你必须至少知道它是什么，通常无论谁提出它，都有很好的解释方法，

> Redis 是一个开源的(BSD 许可的)、内存中的数据结构存储，用作数据库、缓存和消息代理。Redis 提供数据结构，如字符串、散列、列表、集合、带有范围查询的排序集合、位图、超级日志、地理空间索引和流。Redis 具有内置的复制、Lua 脚本、LRU 驱逐、事务和不同级别的磁盘持久性，并通过 Redis Sentinel 和 Redis Cluster 的自动分区提供高可用性。”— redis.io

**我们开始吧，**

于是我开始了对这项技术的探索。

![](img/4fe072c3f4e141cc0e4bf6e9e0f8c3f8.png)

[Thanks for this amazing gif giphy!](https://giphy.com/explore/hobbit-adventure)

**内存中？**在内存中，这意味着它持续存在于您的 RAM 中，这意味着它的访问速度非常快，没有使用 SSD 内存，这在电量不足的情况下可能会很危险。为了解决这一问题，Redis 提供了两种产品 RDB (Redis 数据库)，它是在指定时间间隔后对所有数据进行的单个时间点快照，以及 AOF(仅附加文件)，它存储所做的每一个操作，以便您可以重建数据，值得注意的是，您可以根据需要混合使用这两种产品来保护您的信息。

**数据结构存储？**这是指其核心数据类型非常类似于编程语言中常见的数据结构，如字符串、列表、字典等，很快，这将使您的数据库能够对数据本身执行非常有趣的功能。

**数据库、缓存、消息代理？**我将解释每一个与 Redis 中的内存工作方式有关的特性，以及这三个特性在 Redis 上使用的合理性。由于您可以在一定的时间间隔后用 RDB 保存完整的数据状态，用 AOF 保存记录数据发生的每一次变化的日志，您可以将它用作数据库，因为它具有 AOF 功能，您可以保存下次加速软件整体功能时可能需要的例程。 由于它在内存中使用自己的数据结构，因此可以进行复杂的操作，从而在两个不同的程序之间实现简单快速的通信，这也使它成为消息代理的一个很好的选择。

现在我将简要介绍它的每一种可用的数据结构！

> 二进制安全字符串，可以使用任何二进制序列作为密钥，从字符串“any”到 hash。
> 
> 列表，在这种特殊情况下，它是使用链表实现的，这意味着它总是需要恒定的时间来将任何东西附加到它的尾部，如果您试图查看它中间的内容，这不是最好的。
> 
> 集合，它是唯一字符串的无序集合，可以对集合执行任何经典的操作，比如检查成员资格。
> 
> 排序集合，虽然它基本上与常规集合相同，但它具有将每个值映射到一个称为 score 的浮点值的特性，使事物能够在调用时根据 score 的值自行组织。
> 
> 散列，它是一个字段值对，如果你调用这个字段，你将得到这个值。
> 
> 位数组，从技术上讲，是在字符串类型上定义的一组面向位的操作。存储信息时，它们可以极大地节省空间。
> 
> Hyperloglogs 是一种概率数据结构，用于计算独特的事物，这种算法的神奇之处在于，您不再需要使用与计算的项目数量成比例的内存量，而是可以使用恒定的内存量。
> 
> 流，类似于日志数据结构*、*，但其操作方式更复杂，*、*，它允许一组客户端协作使用同一消息流的不同部分。

这是一个非常基本的解释，因为这篇文章旨在为那些想要全面了解所有信息的人提供帮助，但是如果你确实想要更多，请查看这篇文章，[这是我找到所有这些信息的地方](https://redis.io/topics/data-types-intro)。

现在，这最后一部分将是关于 Redis 描述末尾提到的每一个特性。

**内置复制？**这是制作多个数据副本并将它们存储在不同位置以提高它们在网络中的整体可访问性的过程。

**Lua 脚本？**Redis 内置了 Lua 解释器。这允许使用 Lua 数据类型，它可以转换成 Redis 数据类型。

**LRU 被驱逐？**最近最少使用，是一系列算法，首先丢弃最近最少使用的项目。这可能是 Redis 如此适合缓存的一个原因。

**交易？**这意味着作为一个完整的整体，每个操作要么成功，要么失败，永远不能只是部分完成

**不同级别的磁盘持久性？**我在内存中解释的内容，[更多信息请点击](https://redis.io/topics/persistence)。

**Redis 哨兵？**支持多个从节点从一个主节点复制数据。这提供了一个备份节点，上面有您的数据，随时可以提供数据。

**Redis 集群？**是一种跨多个实例自动划分数据的方法。它被配置为跨给定数量的 Redis 实例传播数据。

就我个人而言，我对这项技术非常感兴趣已经有一段时间了，我看到了它的巨大潜力，尽管如此，它的复杂性和我的时间不足使我无法真正开始，尽管如此，我写这篇文章是为了所有好奇并希望对所有内容有一个非常浅显的看法的人，浅显到不难阅读，但又足够宽泛，使您可以有一个很好的概述。 希望我用这篇文章做到了这一点，如果你喜欢继续关注我的 Redis 系列，这是一组将来会制作 REDIS 教程的文章，TBA。