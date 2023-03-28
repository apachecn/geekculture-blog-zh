# Redis vs Infinispan vs Memcached

> 原文：<https://medium.com/geekculture/redis-vs-infinispan-vs-memcached-7f7cf3b26522?source=collection_archive---------0----------------------->

## Redis 第 4 集

## Redis、Infinispan 和 Memcached 之间的比较

![](img/ecf5adb7280eb2d36865e1eb6a5f1ca3.png)

Photo by [Raquel Martínez](https://unsplash.com/@fiteka?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们已经在之前的文章中讨论了 Redis [简介](https://sthenusan.medium.com/introduction-to-redis-3e6c3a0083a7)、[数据持久性](https://sthenusan.medium.com/data-persistence-with-redis-52b7d7cdfc53)、[可用性](https://sthenusan.medium.com/availability-with-redis-66611f5a5e2b)。我想在这里讨论 Redis 与其他一些类似技术的比较，比如 Infinispan 和 Memcached。众所周知，Redis 是一个内存数据库。这三种都是一些数据存储技术，完全开源，主要用于提高软件系统的性能和可用性。Redis 简介部分单独写在一篇文章里。你可以通过阅读来了解 Redis 的基本概念。

![](img/da81022fab21ef7e23724ab0e335979e.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Infinispan 是一个内存中的分布式键值存储，可以根据 [Apache 2.0](https://en.wikipedia.org/wiki/Apache_License) 许可使用。Infisipan 是在 JAVA 之上编写的。它通常用作主数据库前面的分布式缓存系统，但也可以用作临时数据(如用户会话数据)的存储。内存存储的主要优势之一是性能因素。Infinspan 也像 Redis 一样具有极高的性能。Infinispan 中也提供数据持久性。如果任何服务器出现故障，全部数据都可以恢复。

Infinispan 在节点之间使用无主对等体系结构。对等方法有助于快速纵向和横向扩展。如果其中一个对等体出现故障，则整个系统不会出现大问题。它提高了可用性和可伸缩性。多线程设计确保软件获得系统多核的全部性能。Infinispan 拥有广泛使用的编程语言的客户端，如 Java、Python、Node JS 等。Infinispan 提供集群支持。从安全角度来看，Infinispan 中提供了传统的用户 ID 密码机制、证书方法、Kerberos、基于令牌的安全性(OAuth)。Infinispan 可以与 AWS、Azure、Google Cloud 和 Kubernetes 集成。

Memcached 是一个开源、高性能、分布式内存对象缓存系统。Memcached 也是一个键值数据存储。对于数据库负载高的网站来说，这是非常有效的。与 Redis 不同，Memcached 不支持更丰富的数据结构。Redis 只使用单核，而 Memcached 使用多核。在大多数情况下，当以内核衡量时，Redis 在小规模数据中的性能要高于 Memcached。对于存储大数据，Memcached 的性能比 Redis 好。然而，Redis 也为存储大数据做了一些优化。

集群支持提高了 Memcached 的可用性。到目前为止，Memcached 还没有提供自动故障转移功能。Memcached 不支持数据持久性。如果整个服务器出现一次故障，所有存储的数据都会丢失。Memcached 拥有广泛使用的编程语言的客户端，如 Java、Python、C++。访问控制的 Memcached 实现比 Redis 和 Infinispan 有一些功能。

# 什么和哪里

![](img/88cb98854bdd7309741c52291974bdf1.png)

Photo by [Brendan Church](https://unsplash.com/@bdchu614?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Memcached 和 Infinispan 在伸缩性上比 Redis 更有优势。由于两者都是多线程的，因此通过提供更多的计算资源，它们很容易扩展。您可以使用 Memcached 或 Infinispan，它们可以用在可伸缩性是基本特性的系统中。Memcached 可以用于缓存小型静态数据。Memcached 不能用在持久性至关重要的场景中。

当性能和持久性比其他特性更重要时，Redis 是一个自动选择。Redis 有时因其丰富的数据结构而被使用。在现代数据分析中，Redis 是考虑性能的正确选择。Redis 在大型数据集上也能很好地工作。Infinispan 也很像 Redis。它们之间的主要区别是单线程和多线程。对等架构在可扩展性方面比主从设置更有帮助。Infinispan 可用于管理多核系统中的高工作负载。

Redis、Infinispan、Memcached 都是合理的数据库相关解决方案。一个人可以在不同的情况下超越他人。我们必须根据我们的目的和目标选择一个解决方案。在选择一种技术之前，使用什么和在哪里使用是关键问题。

我相信你很好地理解了今天讨论的主题。如果你有任何问题，请在评论区提出，我会尽快回复。感谢您花费宝贵的时间阅读这篇文章。

***欣赏文章？成为*** [***中等会员***](https://sthenusan.medium.com/membership) ***继续无限制学习。如果你使用上面的链接，我会收到你的一部分会员费，不需要你额外付费。***

参考

[](https://redis.io/) [## 雷迪斯

### 宣布 RedisConf 2021(4 月 20-21 日):内容征集现已开放。Redis 是一个开源软件(BSD 许可)…

redis.io](https://redis.io/) [](https://memcached.org/) [## memcached——分布式内存对象缓存系统

### 免费开源、高性能、分布式内存对象缓存系统，本质上是通用的，但旨在用于…

memcached.org](https://memcached.org/) [](https://infinispan.org/) [## 内存中分布式数据存储

### 在不同地理位置运行的 Infinispan 群集可以形成全局群集，以跨…

infinispan.org](https://infinispan.org/) [](https://alibaba-cloud.medium.com/redis-vs-memcached-in-memory-data-storage-systems-3395279b0941) [## Redis 与 Memcached:内存数据存储系统

### Redis 和 Memcached 都是内存中的数据存储系统。Memcached 是一个高性能的分布式内存缓存…

alibaba-cloud.medium.com](https://alibaba-cloud.medium.com/redis-vs-memcached-in-memory-data-storage-systems-3395279b0941)