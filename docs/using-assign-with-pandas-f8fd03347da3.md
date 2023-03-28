# 对熊猫使用赋值

> 原文：<https://medium.com/geekculture/using-assign-with-pandas-f8fd03347da3?source=collection_archive---------23----------------------->

## 更好的完成工作的方法

在 Pandas 中，`assign`函数是一个基于现有列或其他数据在`DataFrame`中创建新列的有用工具。`assign`函数返回一个添加了列的新的`DataFrame`，所以它不会就地修改原来的`DataFrame`。

![](img/20771f9efb56427358c4958d17697a78.png)

Photo by [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

要使用`assign`功能，您需要导入`pandas`库: