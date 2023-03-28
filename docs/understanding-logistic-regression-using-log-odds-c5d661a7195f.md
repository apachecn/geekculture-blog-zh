# 使用对数优势理解逻辑回归

> 原文：<https://medium.com/geekculture/understanding-logistic-regression-using-log-odds-c5d661a7195f?source=collection_archive---------28----------------------->

![](img/bd39e09080dcd076981098422743d942.png)

Photo by [Shuangyuan Wei](https://unsplash.com/@sharon_sy_wei?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@sharon_sy_wei?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当谈到逻辑回归时，我不得不调整我的思维，因为它模拟的是概率而不是均值，而且它涉及非线性转换。在本文中，我将解释对数概率在数学上对逻辑回归的解释，并使用真实数据运行一个简单的逻辑回归模型。