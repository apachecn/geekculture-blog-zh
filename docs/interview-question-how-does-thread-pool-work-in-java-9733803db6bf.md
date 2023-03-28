# 面试问题:Java 中线程池是如何工作的？

> 原文：<https://medium.com/geekculture/interview-question-how-does-thread-pool-work-in-java-9733803db6bf?source=collection_archive---------1----------------------->

在我们深入这个问题之前，让我们看一下 ThreadPoolExecutor 类的一些关键字段:

*   **corePoolSize** :保持存活的最小工人数。
*   **maximumPoolSize** :池子的容量。
*   **keepAliveTime** :等待工作的空闲线程超时，单位为纳秒。
*   **工作队列**:用于保存任务并移交给工作线程的队列。

## 这些字段是如何链接的？