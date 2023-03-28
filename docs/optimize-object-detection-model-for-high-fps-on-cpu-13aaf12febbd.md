# 针对 CPU 高 FPS 优化对象检测模型

> 原文：<https://medium.com/geekculture/optimize-object-detection-model-for-high-fps-on-cpu-13aaf12febbd?source=collection_archive---------4----------------------->

![](img/11746429c37fa96df36ea3c1989aaa0d.png)

Photo by [Jeremy Bezanger](https://unsplash.com/@jeremybezanger?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

根据研究，数据科学家构建的模型中有 90%从未投入生产。这是因为生产环境有几个限制。在某些情况下，可能是推理时间，也可能是硬件。因此，为了准备好模型，我们需要首先考虑我们将用于生产的模型本身。为了确定不同的型号…