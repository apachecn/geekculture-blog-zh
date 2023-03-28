# NumPy 怎么会这么快？

> 原文：<https://medium.com/geekculture/how-is-numpy-so-damn-fast-edaef071a5b6?source=collection_archive---------15----------------------->

简答，是用 C 写的，长答…

![](img/9cb5976590cf30c8ff14300608a9a8ea.png)

Photo by [Watt A Lot](https://unsplash.com/@wattalot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我们谈论 NumPy 数组(np 数组)的效率时，我们到底指的是什么？有两个独立的问题:

*   内存使用效率。
*   处理效率。

# 为什么 NumPy 数组比 Python 列表更高效？