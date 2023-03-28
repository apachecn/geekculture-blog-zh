# 通过具体的 Python 示例了解偏差-方差误差

> 原文：<https://medium.com/geekculture/understanding-the-bias-variance-error-with-specific-python-examples-145bd3255cfd?source=collection_archive---------32----------------------->

![](img/f1471c4d36a7d1837c3e3f5d60b18fa5.png)

Photo by [Zuzana Ruttkay](https://unsplash.com/@zuzi_ruttkay) on [Unsplash](https://unsplash.com)

# 介绍

在之前的 [**文章**](https://towardsdatascience.com/simple-mathematical-derivation-of-bias-variance-error-4ab223f28791) 中，我推导出了将*测试数据*中的总误差与偏差和方差误差联系起来的等式。该等式由下式给出

其中我在训练数据 ***中“学习”了向量参数 ***θ*** 后，计算了测试数据代价函数的期望值。*** 如果你还没有…