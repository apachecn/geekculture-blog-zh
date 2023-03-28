# Python 中的预测功耗分数实现

> 原文：<https://medium.com/geekculture/predictive-power-score-implementation-in-python-70558bf91f45?source=collection_archive---------8----------------------->

![](img/459e0bd454551cb7d4c898f204329e34.png)

Photo by [Rhett Wesley](https://unsplash.com/@rhett__noonan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

PPS 或 ppscore 库是一个不对称的、数据类型不可知的分数，可以检测两个列之间的线性或非线性关系。得分范围从 0(无预测能力)到 1(完美预测能力)。它可以用作相关性(矩阵)的替代方法。在这个博客中，我将讨论如何使用这个库，并给出我对它的功能的想法。要开始，您需要…