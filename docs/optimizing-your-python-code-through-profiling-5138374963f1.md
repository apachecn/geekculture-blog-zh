# 通过分析优化您的 Python 代码

> 原文：<https://medium.com/geekculture/optimizing-your-python-code-through-profiling-5138374963f1?source=collection_archive---------34----------------------->

不久前，我建立了一个 web scrapper，从 web API 下载产品信息。如你所知，网络上的每件产品都与一张或多张图片相关联，不幸的是，我不得不下载这些图片并存储在我的本地机器上。有趣的是，当你开始从网上下载一堆图片时，你最终会得到很多重复的图片，这需要空间和时间来搜索。所以我决定写几个函数，去掉所有重复的部分。在这个过程中，我认为这将是一篇向你展示你如何…