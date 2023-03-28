# 5 种先进的矢量化技术提高了 Python 性能

> 原文：<https://medium.com/geekculture/5-advanced-vectorisation-techniques-for-improved-python-performance-f76e37f9f522?source=collection_archive---------11----------------------->

使用 NumPy 来加速您的代码。

![](img/1d2e47d423ea71d4af00d276811aa4f6.png)

Photo by [Charlotte Coneybeer](https://unsplash.com/@she_sees?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

NumPy 矢量化在一次调用中对整个数组应用一个函数。例如:

```
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
u = np.sqrt(a)  # [1\. 1.414 1.732]
v = a + b       # [5 7 9]
```