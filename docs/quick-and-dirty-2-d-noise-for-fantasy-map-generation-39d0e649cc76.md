# 快速和肮脏的 2-D 噪音的幻想地图生成

> 原文：<https://medium.com/geekculture/quick-and-dirty-2-d-noise-for-fantasy-map-generation-39d0e649cc76?source=collection_archive---------38----------------------->

![](img/8683e4b666533493bed686017b71a93d.png)

曾经想要创建像这样的酷的自定义幻想地图吗？它实际上非常简单，只有几行 python 代码。下面我会带你看它是怎么做的。不在乎怎么做？直接跳到代码:[这里](https://github.com/eric-robertson/fantasy-map-gen)

首先，这一切都源于随机生成的数据。Numpy 为随机生成的 2D 数组提供了一些不错的功能，比如: