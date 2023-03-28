# 看看 sklearn 的新估计器，自我训练分类器

> 原文：<https://medium.com/geekculture/a-look-at-sklearns-new-estimator-the-selftrainingclassifier-5a8628276abc?source=collection_archive---------7----------------------->

![](img/062e431e1020a06671a44883552de7e0.png)

Python 的首要机器学习库 sklearn 有一个新的估计器，称为 SelfTrainingClassifier。这个类允许给定的监督分类器作为半监督分类器，允许它从未标记的数据中学习。它通过迭代预测未标记数据的伪标记并将它们添加到训练集中来实现这一点。