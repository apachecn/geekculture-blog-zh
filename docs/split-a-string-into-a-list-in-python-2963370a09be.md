# 在 Python 中将一个字符串拆分成一个列表

> 原文：<https://medium.com/geekculture/split-a-string-into-a-list-in-python-2963370a09be?source=collection_archive---------16----------------------->

## 了解如何在 Python 中将一个字符串拆分成一个列表

![](img/3dd221e7ddec6f3cc824bf91e761b833.png)

Photo by [Alex Chumak](https://unsplash.com/@ralexnder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将单个字符串拆分成一个列表

我们可以使用`[split](https://docs.python.org/3/library/stdtypes.html#str.split)`方法从单个字符串中创建一个字符串列表:

```
str.split(sep=None, maxsplit=-1)
```

`str`是将被拆分或用于创建列表的字符串。`sep`是…