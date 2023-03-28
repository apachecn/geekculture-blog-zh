# Pyspark 协同过滤推荐系统

> 原文：<https://medium.com/geekculture/recommender-system-using-collaborative-filtering-in-pyspark-b98eab2aea75?source=collection_archive---------8----------------------->

## 基于交替最小二乘(ALS)算法的协同过滤及其在 Pyspark 中的实现

![](img/f54a88648270b07825da72d5eeb8140f.png)

Photo by [Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

*“推荐系统基于用户的历史行为做出预测*”你有没有注意到网飞是如何…