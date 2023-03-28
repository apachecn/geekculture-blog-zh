# 输出到文件的 Python 多重处理

> 原文：<https://medium.com/geekculture/python-multiprocessing-with-output-to-file-a6748a27ed41?source=collection_archive---------4----------------------->

![](img/90a709ab65f50eb3e4fe767988340275.png)

Photo by [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

每当我们可以将一个大问题分割成多个小块并并行运行时，就会想到 Python 多处理模块。最近，我遇到了一个场景，我需要将并行化的结果写入输出文件，而将写入文件逻辑添加到 worker 函数中的直接方法并不能像预期的那样工作，因为当多个 worker 访问输出时会出现一些并发问题…