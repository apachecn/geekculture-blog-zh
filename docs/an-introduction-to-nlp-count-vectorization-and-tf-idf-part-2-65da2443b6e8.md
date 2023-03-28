# NLP 计数向量化和 TF-IDF 介绍(第 2 部分)

> 原文：<https://medium.com/geekculture/an-introduction-to-nlp-count-vectorization-and-tf-idf-part-2-65da2443b6e8?source=collection_archive---------20----------------------->

让我们一起探索 TF-IDF。

![](img/2793cd4879b9976f49e95f5c999a7136.png)

Photo by [Dan LeFebvre](https://unsplash.com/@danlefeb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在[第 1 部分](https://machinelearningabc.medium.com/an-introduction-to-nlp-count-vectorization-and-tf-idf-part-1-2f979f66f5b2)引入字数矢量化之后。让我们对 TF-IDF 做一个快速的&深入探究。

考虑一个大的文档语料库包含在文档中非常频繁出现但几乎没有有用意义的标记；诸如“是”“开”“到”等标记。机器学习算法可能…