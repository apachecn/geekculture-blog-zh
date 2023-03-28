# 科技巨头是如何设计和实现分布式 ID 生成器的

> 原文：<https://medium.com/geekculture/how-do-tech-giants-design-and-implement-a-distributed-id-generator-bd618803035f?source=collection_archive---------3----------------------->

## 设计可扩展系统

![](img/444ba55b278e5eef47f055c95d4c19f3.png)

Photo by [Noah Näf](https://unsplash.com/@noahdavis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

由于现在太多的系统从小型到大型都需要唯一的全局标识符，这是分布式计算中需要立即解决的一个重要任务。

> 许多系统现在需要唯一的全球标识符，如社会…