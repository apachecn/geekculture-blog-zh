# 使用 tf.data.Dataset 通过过采样创建不平衡数据集

> 原文：<https://medium.com/geekculture/creating-an-imbalanced-dataset-with-oversampling-using-tf-data-dataset-f63e959ba824?source=collection_archive---------13----------------------->

在本文中，我总结了 tensorflow 的实现，用于 1)创建不平衡数据集，2)使用 *tf.data.Dataset* 对未充分表示的样本进行过采样。

这篇文章是针对谁的？你想要

1.  使用 tf.data.Dataset 类
2.  创建不平衡的数据集
3.  想要对不平衡数据集中代表性不足的样本进行过采样
4.  应用图像和批次级数据增强
5.  根据给定数据集创建分割(训练、验证)

工具和数据集规格:

*   keras ( *v2.4.3*
*   tensorflow ( *v2.2.0* )后端
*   来自 *tf.data.Dataset* 管道的 *cifar-10* 数据集

**TL；下面的 DR** 是满足这些需求的最简单的代码片段。您可以复制和试验，或者向下滚动来阅读每个代码块正在做什么的简介。

Code snippet by the author

**简介**

1.  *allowed_id_list()* :随机选择不平衡类样本的“*不平衡 _ 样本 _ 大小*”(: =*筛选 _ 标签*
2.  *_filter_list():* 只过滤出不平衡类允许的样本。
3.  使用 *ds.take()* 和 *ds.skip()* 创建列车/val 分割。
4.  *oversample_classes()* ，重复不平衡类的样本。当前的实现为来自少数类的样本返回“2”，为其余的样本返回“1”。更多可能性，请参考下文

[](https://github.com/tensorflow/tensorflow/issues/14451#issuecomment-343587112) [## 数据集 API 问题#14451 tensorflow/tensorflow 中的过采样功能

### 你好，我想问一下目前数据集的 API 是否允许实现过采样算法？我处理…

github.com](https://github.com/tensorflow/tensorflow/issues/14451#issuecomment-343587112) 

5. *map_fn()* 和 *batch_map_fn()* 可分别用于放大单幅图像和整批图像

**注:**

1.  将数据分割成*验证*和*训练*数据集后，确保执行*过采样 _ 类*映射。否则有数据泄露的可能。同样适用于 *ds.shuffle()* 。
2.  不在 *ds.shuffle()* 中设置“*reshuffle _ each _ iteration = True*”会(在一定程度上)限制数据集在子集内的重排。

这涵盖了实现不平衡数据集或具有过采样类的数据集的基础知识。如果你觉得我错过了什么，请留下评论或分享。

如果你有兴趣进一步探索 *tf.data.Dataset* 类，可以看看我下面这篇文章。

 [## 将 tf.data.Dataset 与 ImageDataGenerator 结合用于图像对

### 在本文中，我将介绍如何在 keras 的 ImageDataGenerator 上实现 tf.data.Dataset 类来创建一个…

anujarora04.medium.com](https://anujarora04.medium.com/combining-tf-data-dataset-with-imagedatagenerator-for-image-pairs-698e2a2e71fd) 

如果你觉得这些故事很有价值，并愿意支持我成为一名作家，请考虑**关注我**或[注册 成为媒体会员。](https://anujarora04.medium.com/membership)