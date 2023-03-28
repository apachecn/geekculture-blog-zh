# 单一模式

> 原文：<https://medium.com/geekculture/singleton-pattern-126281458ee5?source=collection_archive---------29----------------------->

![](img/40c2a339dea88def77d49c8a5e98eaa1.png)

Photo by [Noah Näf](https://unsplash.com/@noahdavis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

单例对象是在整个系统中只有一个实例的对象。潜在的单例包括:

*   一个记录系统。当系统执行时，系统的各个部分可能需要写日志消息。这些通常需要写到同一个文件中，所以通常只需要整个系统使用的一个 logger 对象。
*   数据库连接。系统…