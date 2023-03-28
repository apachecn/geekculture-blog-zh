# MongoDB 关系:嵌入与链接——利弊

> 原文：<https://medium.com/geekculture/mongodb-relationships-embedding-vs-linking-pro-and-cons-5f7583e655ab?source=collection_archive---------2----------------------->

## 使用弹簧靴进行性能比较

当在 MongoDB 中实现文档之间的关系时，我们可以遵循两条主要路径，嵌入或链接关系(也称为索引、规范化或引用)。

![](img/7f968c9f8e9aa308390e788368925e6d.png)

Photo by [Alvaro Reyes](https://unsplash.com/@alvarordesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

以一个**帖子**为例，可以有一个或多个**评论**与之相关