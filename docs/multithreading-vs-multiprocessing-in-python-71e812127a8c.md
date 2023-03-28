# Python 中的多线程与多处理

> 原文：<https://medium.com/geekculture/multithreading-vs-multiprocessing-in-python-71e812127a8c?source=collection_archive---------13----------------------->

## 第一部分从理论上谈

![](img/42242317a55667c705f263739b35f75a.png)

Photo by [Mike van den Bos](https://unsplash.com/@mike_van_den_bos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在开始讨论多线程和多处理之前，我先简单介绍一下什么是计算机和程序中的**进程**和**线程**:

*   一个进程就是我们所说的程序，这个程序已经和它运行所需的所有资源一起被加载到内存中。
*   线程是执行的单位…