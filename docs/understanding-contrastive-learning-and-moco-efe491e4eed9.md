# 理解对比学习和 MoCo

> 原文：<https://medium.com/geekculture/understanding-contrastive-learning-and-moco-efe491e4eed9?source=collection_archive---------4----------------------->

## 在对比学习中什么是重要的，它能有什么帮助

![](img/7a8924baac3a72da786fea171420b85d.png)

Photo by [Nadir sYzYgY](https://unsplash.com/@nadir_syzygy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了获得高精度和良好的性能，深度学习模型需要大量的标记数据。出于学术目的，我们可以使用来自 ImageNet 等公共数据集的数百万标记数据。然而，要在网络中收集大量的标记数据是非常困难的