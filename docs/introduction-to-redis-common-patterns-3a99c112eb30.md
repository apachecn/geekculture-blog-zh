# Redis 通用模式介绍

> 原文：<https://medium.com/geekculture/introduction-to-redis-common-patterns-3a99c112eb30?source=collection_archive---------8----------------------->

![](img/8dd9267035e32f0fb7f29fd6d3f0a0e0.png)

[Redis(远程字典服务器)](https://redis.io/)是一个内存中的数据存储，我们可以将其用作数据库、缓存和消息代理。Redis 最明显的用例是作为服务器端的缓存存储。除了缓存，还有一些模式可以帮助我们解决常见的问题。

# **限速**

您有一个公共端点，并且希望根据客户端 IP 地址限制对端点的访问。假设我们希望将访问限制为每分钟 100 个请求。通过使用内置的 Redis [GET](https://redis.io/commands/get) 和 [INCR](https://redis.io/commands/incr) 函数，我们可以为我们的端点构建速率限制器函数。流程非常简单:

*   根据客户端 IP 和当前分钟创建一个密钥，即 10.1.89.100:15。
*   通过对我们的端点使用每个请求的键，从 Redis 获取值。
*   如果该值大于 100，则中断该过程。
*   如果该值小于或等于 100，则继续该过程。
*   使用基于键的 INCR 增加值。
*   要清除 Redis 存储中未使用的数据，使用 [EXPIRE](https://redis.io/commands/expire) 将密钥到期时间设置为 1 分钟

好，让我们在端点上实现速率限制器:

我们创建“GET /pokemons”端点来返回 Pokemon 数据数组，并将端点的使用限制为每分钟 5 个请求。对于每个请求，我们使用客户端 IP 作为密钥，并增加计数器。当计数器大于 5 时，我们立即响应 429 HTTP 状态，因此客户端应该再等待一分钟来执行它们的下一个请求。

# **锁定**

有时，我们希望我们的应用程序在同一时间做完全相同的工作，我们需要阻塞其他请求，直到前一个请求完成。

假设我们有“POST /generate_reports”端点，该端点负责规范化数据库上的数据，并基于规范化的数据创建报告。我们希望一个用户只能同时生成一个报告。通过将 Redis [SET](https://redis.io/commands/set) 与 NX 和 EX 命令一起使用，我们可以创建一个锁来保护我们的资源进行多路访问。下面是创建我们的锁定机制的步骤:

*   基于用户 ID 或某个标识符创建一个密钥。
*   生成一个随机字符串作为值。
*   定义一个生存时间(TTL)来防止我们的锁死锁，所以当客户端无法释放锁时，密钥将根据我们的 TTL 被清除。
*   通过使用先前的密钥、值和 TTL，使用“SET {KEY} {VALUE} NX EX {TTL}”命令将其放入 Redis。NX 选项只在键不存在时设置键，当键已经存在时返回 nil。
*   如果前面的命令没有返回 nil，则运行报告生成过程，否则立即中断。
*   当报告生成后，我们需要释放锁。为了避免密钥被另一个客户端删除，在前面的步骤中，我们创建了一个不可猜测的随机字符串作为值。因此，我们将创建一个脚本，仅在值匹配时删除键。

```
if redis.call("GET",KEYS[1]) == ARGV[1]
then
    return redis.call("DEL",KEYS[1])
else
    return 0
end
```

*   在锁被释放后，我们可以接受另一个请求。

下面是我们锁实现的例子:

Redis 的作者 Redis 提出了更健壮的锁算法叫做 [Redlock](https://redis.io/topics/distlock) 。我已经在这个[帖子](https://syafdia.medium.com/implementing-redlock-on-redis-for-distributed-locks-a3cfe60d4ea4)上写了关于红锁实现的文章。

# **工作队列**

基于[维基百科](https://en.wikipedia.org/wiki/Job_queue)的作业队列定义:

> 在[系统软件](https://en.wikipedia.org/wiki/System_software)中**作业队列**(有时是**批处理队列**)，是一个由[作业调度器](https://en.wikipedia.org/wiki/Job_scheduler)软件维护的数据结构，包含要运行的作业。用户将他们想要执行的程序“作业”提交到[批处理队列](https://en.wikipedia.org/wiki/Batch_processing)。

简而言之，通过使用作业队列，我们可以将有关要执行任务的信息放入队列中，从而在多个进程之间使用异步通信。

队列上的操作是先进先出(FIFO)。Redis 内置了列表数据结构。通过在列表上使用 [BRPOP](https://redis.io/commands/brpop) 和 [LPUSH](https://redis.io/commands/lpush) 命令，我们可以实现自己的作业队列。

首先，我们需要创建一个 worker，从队列中提取作业

*   我们使用“BRPOP {QUEUE_NAME} 0”命令从队列中提取作业，该命令将阻塞我们的程序，直到它接收到作业。
*   由于前一个命令在接收到一个作业后会被终止，所以我们应该把它放在无限循环里面。
*   收到作业后，处理该作业。

就这样，接下来我们将创建一个处理程序来将作业推入队列

*   我们只需创建一个 HTTP 端点“POST /send_welcome_email ”,它负责接收电子邮件，并将电子邮件放入队列中，以便工作人员可以异步发送电子邮件。
*   使用“LPUSH {QUEUE_NAME} {EMAIL}”将电子邮件推送至队列。

尝试在终端的不同选项卡上运行 worker 和 HTTP 处理程序。当您向/send_welcome_email 端点发送包含电子邮件数据的发布请求时，我们的工作人员将立即提取作业并处理该电子邮件地址。

酷，我们刚刚实现了我们自己的作业队列系统，但这还不是生产就绪。如果你想通过 Redis 在产品上使用 queue，你可以使用 [Sidekiq](https://github.com/mperham/sidekiq) ，这篇[文章](https://longliveruby.com/articles/how-sidekiq-really-works)解释了 Sidekiq 如何在内部工作。

# **酒馆 Sub**

我只是再次引用[维基百科](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern)XD

> 在[软件架构](https://en.wikipedia.org/wiki/Software_architecture)中，**发布-订阅**是一种[消息传递模式](https://en.wikipedia.org/wiki/Messaging_pattern)，其中[消息](https://en.wikipedia.org/wiki/Message_passing)的发送者(称为发布者)不将消息编程为直接发送给特定接收者(称为订阅者)，而是将发布的消息分类，而不知道可能有哪些订阅者(如果有的话)。类似地，订阅者表达对一个或多个类别的兴趣，并且仅接收感兴趣的消息，而不知道存在哪些发布者(如果有的话)。

因此，基本上，发布订阅是一种模式，其中发布者向通道发送消息，订阅者从通道接收消息。而发布者并不关心订阅者是谁，订阅者的目的是什么。

使用 pub sub 的 Redis 命令非常简单，如果我们想要订阅一个频道，我们调用 [SUBSCRIBE](https://redis.io/commands/subscribe) ，如果我们想要向给定的频道发布消息，我们调用 [PUBLISH](https://redis.io/commands/publish) 。

既然 Redis 为我们提供了内置的发布订阅实现，那么让我们来关注一下如何使用它。假设我们有一个电影院应用程序，我们有一个在电影院预订座位和处理付款的功能。对于这个特性，我们开发了 3 个服务，姑且称之为:

*   HTTP 座位管理器应用程序
*   Pub Sub 支付应用
*   酒吧副座经理应用程序

当用户在电影院观看电影时，他们将访问我们的网页，并通过 HTTP Seat Manager App 选择座位，当用户的座位被阻止时，它将发布到 BLOCK_SEAT_SUCCESS 频道。

我们的 Pub Sub 支付应用程序将侦听 BLOCK_SEAT_SUCCESS 通道，当它通过这些通道收到消息时，它将通过“make_payment”功能进行支付，如果支付成功，一条消息将发布到 MAKE_PAYMENT_SUCCESS 通道。当支付失败时，它会通过 MAKE_PAYMENT_FAILED 通道发布另一条消息。

Pub Sub 支付 App 有“退款”功能，以防有 ALLOCATE_SEAT_FAILED 频道的消息。因为当“allocate_seat”功能出错时，我们的 Pub Sub Seat Manager 应用程序将通过此通道发布一条消息。

Pub Sub Seat Manager 应用程序将侦听 MAKE_PAYMENT_SUCCESS 通道，如果它通过该通道接收到数据，它将调用“allocate_seat”函数为用户分配座位。并且在从 MAKE_PAYMENT_FAILED 或 ALLOCATE_SEAT_FAILED 通道收到消息的情况下，它具有“unblock_seat”功能，因此可以为其他流程释放座位。

是的，这是我们使用 Redis 发布订阅特性的例子。在单独的终端上运行它，你可以看到输出。并尝试调整参数，使其失败或成功，并查看其他进程将基于订阅的通道运行。

我们刚刚使用 Redis 实现了一些常见的模式，但是这些模式不仅限于本文，我们可以实现地理空间、IP 范围索引、全文搜索、分区索引、Bloom Filter 等。你可以在这个库的[上看到这篇文章的完整源代码。](https://github.com/syafdia/programming-sample/tree/master/RedisPattern)

谢谢

**参考**

*   [https://redis.io](https://redis.io/)
*   [https://redislabs.com/redis-best-practices/introduction](https://redislabs.com/redis-best-practices/introduction/)
*   [https://longliveruby.com/articles/how-sidekiq-really-works](https://longliveruby.com/articles/how-sidekiq-really-works)