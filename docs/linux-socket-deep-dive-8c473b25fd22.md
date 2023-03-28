# Linux —套接字深度开发

> 原文：<https://medium.com/geekculture/linux-socket-deep-dive-8c473b25fd22?source=collection_archive---------1----------------------->

## 高级 Linux 网络故障排除知识

![](img/5f28111751341914af2671844623204f.png)

我在以前的文章中谈到了 TCP 和 UDP 协议。在本节中，让我们探索基于 TCP 和 UDP 协议的套接字编程。

建立套接字时，应该设置哪些参数？Socket 编程进行的是端到端的通信，往往不在乎有多少局域网和…