# Node.js 默认内存设置

> 原文：<https://medium.com/geekculture/node-js-default-memory-settings-3c0fe8a9ba1?source=collection_archive---------3----------------------->

![](img/8b9f11db426bee89c9df747dd67169ac.png)

Photo by [Doug Maloney](https://unsplash.com/@dougmaloney?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 💥分配失败—进程内存不足

如果您看到了上面的错误，这意味着您的 NodeJS 应用程序已经耗尽了内存，它消耗了比分配的更多的内存，并最终导致它杀死自己。

发生这种情况的场景之一是当应用程序批处理一个非常大的数据集，并且数据…