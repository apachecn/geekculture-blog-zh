# Linux 内存:缓冲区与缓存

> 原文：<https://medium.com/geekculture/linux-memory-buffer-vs-cache-44d8a187f310?source=collection_archive---------1----------------------->

## 你真的了解缓冲区和缓存的区别吗？

![](img/a6fa6ded2ff7c70bfd58ef4a20354e59.png)

假设您已经知道“缓冲区”和“缓存”的定义:

*   **缓冲区**是原始磁盘块的临时存储，也就是缓存数据写入磁盘，通常不会很大(20MB 左右)。通过这种方式，内核可以集中分散的写入并优化磁盘写入…