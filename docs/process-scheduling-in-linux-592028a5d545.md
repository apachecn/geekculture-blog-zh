# Linux 中的进程调度

> 原文：<https://medium.com/geekculture/process-scheduling-in-linux-592028a5d545?source=collection_archive---------0----------------------->

## 过程的指挥者

![](img/c28b25ee7f45cc355addddac5613c0d9.png)

Photo by [William Milliot](https://unsplash.com/@wmilliot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 加入[中级会员](https://eliran9692.medium.com/membership)阅读我所有的作品(我收取费用，不增加你的成本)

**调度**是分配*资源*执行*任务的动作。* 我们将主要关注调度，其中我们的*资源*是一个处理器或多个处理器，而*任务*将是一个需要执行的线程或进程。