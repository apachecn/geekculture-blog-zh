# 如何使用 sklearn 的 as_frame 参数

> 原文：<https://medium.com/geekculture/how-to-use-sklearns-as-frame-parameter-380942abbd7d?source=collection_archive---------8----------------------->

![](img/f671c48908c324a8c7936244c46970fc.png)

我一直在研究 sklearn 的玩具数据集，发现 0.23 版本的 sklearn 库有了新的参数 as_frame。

如果 as_frame 参数为真，那么 X 和 y 变量将出现在 pandas 数据帧中。如果 as_frame 为 false，数据将显示在数组中。因为文档没有给出正确使用 as_frame 的例子…