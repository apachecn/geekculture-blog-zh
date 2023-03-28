# 写出更好 Python 的 18 种方法

> 原文：<https://medium.com/geekculture/18-ways-to-write-better-python-1d4556b444c1?source=collection_archive---------3----------------------->

## #1 不要使用。append()重复使用。扩展()

![](img/cd3d1fe24f6bfac0a29a8f41034cba73.png)

Photo by [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.使用 x.extend(…)而不是重复调用 x.append()

当向列表追加多个值时，可以使用。extend()方法将 iterable 添加到现有列表的末尾。这样，你就不用打电话了。在每个元素上追加():