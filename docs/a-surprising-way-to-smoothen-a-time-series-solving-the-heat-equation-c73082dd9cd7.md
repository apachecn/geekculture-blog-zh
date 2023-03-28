# 一个令人惊讶的平滑时间序列的方法——求解热量方程！

> 原文：<https://medium.com/geekculture/a-surprising-way-to-smoothen-a-time-series-solving-the-heat-equation-c73082dd9cd7?source=collection_archive---------1----------------------->

## 一个平滑金融市场数据的应用程序

![](img/cff89ba8e9cbeaa4745638a79eced243.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当开发用于时间序列模式识别的机器学习模型时，我喜欢预处理我的数据，以便它更平滑。这有时有助于加速收敛，因为算法可以更好地理解任何趋势或特征。我有…