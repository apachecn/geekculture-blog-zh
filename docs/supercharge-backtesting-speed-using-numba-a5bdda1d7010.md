# 使用 Numba 加速回测速度

> 原文：<https://medium.com/geekculture/supercharge-backtesting-speed-using-numba-a5bdda1d7010?source=collection_archive---------5----------------------->

## 在 Python 回溯测试中挤出额外性能的简单方法

![](img/33421042b6601636a5a0cc766f785f3b.png)

Photo by [Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在交易世界中拥有数据科学技能的好处在于，你可以定制自己的策略，并很快对它们进行回溯测试。然而，如果你是:

*   大样本测试。
*   表演更多…