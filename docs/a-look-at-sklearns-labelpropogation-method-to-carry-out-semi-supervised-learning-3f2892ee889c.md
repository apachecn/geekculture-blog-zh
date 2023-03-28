# sklearn 的 LabelPropogation 方法实现半监督学习

> 原文：<https://medium.com/geekculture/a-look-at-sklearns-labelpropogation-method-to-carry-out-semi-supervised-learning-3f2892ee889c?source=collection_archive---------18----------------------->

Python 的首要机器学习库 sklearn 到目前为止有三个功能来执行半监督学习，即自训练分类器、标签推广和标签传播。

半监督学习是训练数据中的一些标签没有被标记的情况。Sklearn 的三个半监督估计器能够利用未标记的数据来更好地捕捉基础数据分布的形状，并更好地推广到新样本。这些算法…