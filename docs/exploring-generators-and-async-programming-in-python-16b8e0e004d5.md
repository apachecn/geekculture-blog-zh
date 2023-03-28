# 探索 Python 中的生成器和异步编程

> 原文：<https://medium.com/geekculture/exploring-generators-and-async-programming-in-python-16b8e0e004d5?source=collection_archive---------9----------------------->

最近，我一直在探索异步世界，主要是 asyncio 库，偶然发现了一个有趣的发现。大多数正在进行的异步工作都是基于协程、未来对象、生成器等等。因为我是一个喜欢深入挖掘的人，所以我决定探索 Python 生成器以及它们与异步世界的关系。

![](img/3ecedbdce6f20f2c4fc3eb9301ee08d5.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)