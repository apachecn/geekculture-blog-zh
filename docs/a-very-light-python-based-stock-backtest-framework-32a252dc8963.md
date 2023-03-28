# 一个非常轻量级(基于 Python)的股票回溯测试框架

> 原文：<https://medium.com/geekculture/a-very-light-python-based-stock-backtest-framework-32a252dc8963?source=collection_archive---------9----------------------->

看看这个。尽情享受吧！

![](img/2a3ab09517692ffe44fc2c7faac16a1b.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 回溯测试框架

下面，你会发现为评估基于股票的策略而开发的第一类*回溯测试器*的最初定义和方法。这里的独特元素是:第一，这个类是从任何模型中抽象出来的，第二，风险价值用于确定每日头寸的大小，以达到风险阈值，第三…