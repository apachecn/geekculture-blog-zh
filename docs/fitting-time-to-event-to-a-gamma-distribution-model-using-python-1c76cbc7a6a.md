# 使用 Python 将“事件时间”数据拟合到伽马分布模型

> 原文：<https://medium.com/geekculture/fitting-time-to-event-to-a-gamma-distribution-model-using-python-1c76cbc7a6a?source=collection_archive---------3----------------------->

以及如何用它来预测未来的观测。

我之前写过一篇[文章](https://federicoriveroll.medium.com/what-is-the-gamma-distribution-used-for-2c3d6300bdbf)，其中我用一个实际的例子解释了什么是伽马分布的**直觉(用伽马建模直到飞机着陆的时间)。**

在这篇文章中，我将通过一个实用的方法来解释如何**将现有数据拟合到伽马分布**，以及如何使用它来进行预测。

# **生成数据**