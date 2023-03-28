# 在 Python 中使用选项

> 原文：<https://medium.com/geekculture/using-optionals-in-python-412c34fa799e?source=collection_archive---------1----------------------->

对任何人说不。

![](img/3a31f26c21a424c3b12ad7a56eda5f88.png)

Photo by [Lacey Williams](https://unsplash.com/@travelwithlace?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

代码中经常会出现不存在所需值的情况。处理这个问题的典型方法是返回`None`，但是可选函数提供了一个更简洁的方法，并且 [optional.py](https://github.com/cbefus/optional.py) 模块提供了一个易于使用的可选函数实现。

# 一个例子

例如，假设我们有一个客户数据库，我们需要一个函数来查找…