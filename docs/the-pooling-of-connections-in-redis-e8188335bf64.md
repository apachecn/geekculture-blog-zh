# Redis 中的连接池

> 原文：<https://medium.com/geekculture/the-pooling-of-connections-in-redis-e8188335bf64?source=collection_archive---------0----------------------->

## Redis 事件

## Redis 在多线程环境中的最佳实践

大家好，

今天我们将学习如何在多线程环境中使用 Redis。如果你是 Redis 的新手，最好先把之前的文章过一遍再进一步阅读。你可以在这里找到 Redis 文章树。

## 数据库连接池

连接池意味着在每次请求连接时重用连接，而不是创建连接。为了便于连接重用，数据库连接的内存缓存(称为连接池)由连接池模块作为一个层来维护。连接池在后台执行，不影响应用程序的编码方式。

![](img/e0fa2ccb81cd8b6d795b4fd5b5f85eb7.png)

Photo by [Steve Harvey](https://unsplash.com/@trommelkopf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 杰迪池

我们要通过 Redis 的 Java 客户端 Jedis 来学习 Redis 连接池。

在多线程环境中使用 Redis 有一些问题。Redis 连接实例是单线程的。如果我们在多个线程之间重用单个 Redis 连接，可能不足以在有限的时间内完成所需的任务。如果我们为一个线程创建一个单独的 Redis 连接，这将增加 Redis 服务器的开销。

我们需要在多线程环境中建立一个连接池来应对这些挑战。这允许我们从多个线程与 Redis 对话，同时仍然获得重用连接的好处。JedisPool 对象是线程安全的，可以同时在多个线程中使用。我们的想法是，从池中取出连接，并在完成后将其释放回池中。该池只需配置一次，可以重复使用多次。

![](img/7bcba3f00b78689377b75f653cb5a0a4.png)

Photo by [Photos by Lanty](https://unsplash.com/@photos_by_lanty?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## Jedis 池配置

在使用池之前，我们需要根据我们的目的相应地配置初始配置。我将逐一解释初始配置的属性。

1.  *maxTotal:* 该属性管理在给定时间可以产生的最大连接数。因为 Jedis 连接不能跨线程共享，所以这个设置会影响应用程序在与 Redis 交互时的并发量。注意，每个连接都有一些内存和 CPU 开销，因此将这个值设置得很高可能会有负面影响。如果没有设置，默认值为 8，对于大多数应用程序来说，这个值太低了。当选择一个值时，要考虑在高负载下可能有多少个对 Redis 的并发调用。
2.  *maxIdle:* 这是连接池中可以空闲而不被快速关闭的最大连接数。如果未设置，默认值为 8。这个值的建议值与 *maxTotal* 相同，有助于在应用程序在短时间内出现大量负载时避免不必要的连接开销。如果一个连接长时间空闲，它仍然会被驱逐，直到空闲连接数达到 *minIdle* 。*迷你轴*解释如下。
3.  *minIdle:* 这是可以立即使用的连接数。即使负载减少，它们仍会保留在池中。如果未设置，默认值为 0。当选择一个值时，考虑您对 Redis 的*稳态*并发请求。例如，如果您的应用程序同时从 10 个线程调用 Redis，那么您应该将这个值设置为至少 10(稍微高一点，以便给您一些空间)。
4.  *blockWhenExhausted:* 这控制线程请求连接时的行为，但是没有空闲的线程，并且池不能创建更多的线程(由于 *maxTotal 配置*)。如果设置为 true，调用线程将在抛出异常之前阻塞 *maxWaitMillis* 。缺省值为 true，我建议将其真正用于生产环境。您可以在测试环境中将其设置为 false，以帮助您更容易地发现为 *maxTotal* 使用什么值。
5.  *maxWaitMillis:* 如果调用 JedisPool.getResource()会阻塞，要等待多长时间，以毫秒为单位。默认值为-1，表示无限期阻塞。
6.  *TestOnBorrow:* 控制连接从池中返回之前是否进行测试。默认值为 false。设置为 true 可能会增加对连接波动的恢复能力，但从池中获取连接时也可能会有性能损失。

在池连接设置中设置这些参数之前，我们需要三思。

![](img/ad9634bc2ffe6efb24569b54156136d0.png)

Photo by [Rima Kruciene](https://unsplash.com/@rimakruciene?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 样本池配置设置代码

如果我们不想改变默认值，我们可以保留一些配置。我们可以通过调用下面的代码来获得 Jedis 实例。

```
jedisInstance = getJedisInstance().getResource()
```

我们可以继续使用 *jedisInstance* 对象进行存储和检索。

我相信你已经理解了今天讨论的主题。如果您有任何问题或任何澄清，不要犹豫，通过回复部分与我联系。感谢您花费宝贵的时间阅读这篇文章。

***喜欢这篇文章吗？成为*** [***中等会员***](https://sthenusan.medium.com/membership) ***继续无限制的学习。如果你使用上面的链接，我会收到你的一部分会员费，不需要你额外付费。***

干杯…

![](img/fd1c5847478a6384596a7479479ed2fa.png)

Photo by [Niclas Illg](https://unsplash.com/@nicklbaert?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 参考

[](https://redis.io/) [## 雷迪斯

### 宣布 RedisConf 2021(4 月 20-21 日):内容征集现已开放。Redis 是一个开源软件(BSD 许可)…

redis.io](https://redis.io/) [](https://redislabs.com/blog/connection-pools-for-serverless-functions-and-backend-services/) [## 无服务器功能和后端服务的连接池| Redis 实验室

### 当编写连接到数据库的服务器应用程序时，您经常必须处理连接池，用…

redislabs.com](https://redislabs.com/blog/connection-pools-for-serverless-functions-and-backend-services/)