# Linux 负载平均深度

> 原文：<https://medium.com/geekculture/linux-load-average-deep-dive-60a90849a2dd?source=collection_archive---------4----------------------->

你真的了解负载平均吗？

![](img/2c6278c919357b4031112bd319dafb1b.png)

每当我们发现系统变慢，我们通常做的第一件事就是运行`top`或`uptime`命令来了解系统的负载，例如:

```
$ uptime
02:34:03 up 2 days, 20:14,  1 user,  load average: 0.63, 0.83, 0.88
```

但我想问的是，你真的知道这里输出的每一列是什么意思吗？