# 使用 Python 的集合模块实现高效队列

> 原文：<https://medium.com/geekculture/implement-efficient-queues-using-pythons-collections-module-d07eaf7aafbc?source=collection_archive---------15----------------------->

## 在 Python 的`collections`模块中使用 deque 创建定制队列

![](img/6fbc4123facfd2834bf151f7a27ecbca.png)

Photo by [Emile Guillemot](https://unsplash.com/@emilegt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Python 中的`[collections](https://docs.python.org/3/library/collections.html)`模块除了内置容器外，还实现了一些专门的容器数据类型，`dictionary`、`list`、`set`和`tuple`。`deque`就是其中之一。使用`deque`我们可以高效地创建自定义队列数据结构。