# 后端开发人员工作面试问题— Redis (Pt。1–10)

> 原文：<https://medium.com/geekculture/backend-developer-job-interview-questions-redis-pt-1-10-24219b8e27b4?source=collection_archive---------4----------------------->

![](img/3d501c6c998da5047223f55c7182d042.png)

## 1.Redis 比传统数据库有什么优势？

Redis 不仅支持简单的键值类型数据，还为列表、集合、zset、散列和其他数据结构提供存储。Redis 性能极高，读取数据速度可达 110000 次/秒，写入速度可达 81000 次/秒，Redis 还有发布/订阅、通知、密钥过期等有用的特性。Redis 的另一个优点是 Redis 运行在内存中，但可以持久化到磁盘上。Redis 内存高速缓存上的操作允许更低成本的 CPU 工作。

## **2。【Redis 内存耗尽会怎样？**

如果 Redis 达到配置的最大存储，写操作将返回错误消息(但读操作将正常返回)。或者，我们也可以让 Redis 在将新数据写入数据库的同时消除旧数据。

## 3.Redis 集群中的主从复制模式是什么？

为了使群集在一些节点失败或大多数节点无法通信时仍然可用，群集使用主从复制模型，每个节点将有 N-1 个副本。

## 4.有哪些常见的 Redis 性能问题及解决方法？

1.  为了确保主从复制的速度和连接的稳定性，主机和从机应该在同一个网络上。
2.  尽可能避免主从图结构。最好的做法是像 Master
3.  It is best not to write memory snapshots for Master. If Master writes memory snapshots, the save command schedules the rdbSave function, which will block the work of the main thread. When the snapshot is relatively large, the performance impact will be very large, and the service will be suspended intermittently.

## 5\. What are the ways to delete Redis expired keys?

**1)使用定时器**
一样使用单头链表在设置密钥到期时间的同时，创建一个定时器)。当密钥到期时，让计时器立即执行密钥删除操作。

**2)懒删除**
让密钥过期。每当我们得到一个密钥时，检查该密钥是否已经过期。如果过期，请删除密钥。如果没有，返回密钥。

**3)定期删除**
程序定期检查 Redis 数据库，删除过期的密钥。

## 6.Redis 集群的原理是什么？

1.Redis Sentinal 专注于高可用性。当主设备停机时，它会自动将从设备升级为主设备，继续提供服务。
2。Redis 集群侧重于可伸缩性。当单个 Redis 内存不足时，使用集群进行分片存储。

## 7.Jedis 和 Redisson 对比有什么优缺点？

Jedis 和 Redisson 都是 Redis 的 java 客户端。Jedis 几乎是无依赖的，而 Redisson 需要 Netty、JCache API 和 Project Reactor 作为基本的依赖。Jedis 是一个公开 API 的低级驱动程序，它为 Redis 命令提供了更全面的支持。Redisson 实现了一个分布式和可扩展的数据结构。与 Jedis 相比，Redisson 具有更简单的功能，让开发人员专注于业务逻辑。Jedis 客户端实例不是线程安全的，因此它们需要连接池(每个调用线程一个 Jedis 实例)。Redisson API 本身是线程安全的，需要的资源更少。

## 8.如何设置 Redis 密钥的过期时间和永久有效期？

过期和保留命令。

## 9.与 Redis 事务相关的命令有哪些？

MULTI、EXEC、DISCARD、WATCH 命令。

## 10.Redis 如何优化内存？

尽可能多地使用散列。哈希表使用的内存(也就是说哈希表中存储的数字很小)非常小，所以你要尽可能的把你的数据模型抽象成一个哈希表。例如，如果您的 web 系统中有一个用户对象，请不要为用户名、姓氏、电子邮件地址和密码设置单独的键。相反，您应该将所有用户信息存储在一个哈希表中。

我希望这篇文章对你有所帮助。如果你像我一样渴望学习一些与技术相关的东西或定期反思工作和生活，请关注我的频道，了解我日常工作和生活中的最新灵感。

**接下来:** [后端开发者工作面试问题— Redis ( Pt。11–20)](/@wdn0612/backend-developer-job-interview-questions-redis-pt-11-20-80d7fc7ca928)

演职员表:
[https://stack overflow . com/questions/42250951/redis son-vs-jedis-for-redis](https://stackoverflow.com/questions/42250951/redisson-vs-jedis-for-redis)
[https://zhuanlan.zhihu.com/p/276371544](https://zhuanlan.zhihu.com/p/276371544)