# 基于锁的线程同步与无锁线程同步

> 原文：<https://medium.com/geekculture/lock-based-vs-lock-free-thread-synchronization-cbae710a8ab9?source=collection_archive---------17----------------------->

## 两种线程同步方法的性能比较

![](img/f89d41a35c18950c4ea765253f45391b.png)

Photo by [John Anvik](https://unsplash.com/@redviking509?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

并行编程已经成为所有程序员都应该掌握的必然技能。并行运行的线程通常使用共享资源，原因有几个，例如通信和协调。线程同步是一种避免竞争情况的技术，当…