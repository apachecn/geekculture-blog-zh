# 下一个 NLP 项目可能不需要嵌入层

> 原文：<https://medium.com/geekculture/embedding-layer-might-not-be-necessary-for-your-next-nlp-project-e0727049e7a0?source=collection_archive---------30----------------------->

好吧，这可能有点夸张，但即使你确实需要一个，你也可以试着从一个小的嵌入层开始。

这篇博客文章谈到了嵌入层，在大多数现代神经网络的最开始，它通常将整个词汇映射到一些密集的分布式表示上。

您可能没有意识到这一点，但是嵌入层包含相当多的参数，根据您的任务或数据集，您可能根本不需要它们。这里简单算一下。通常情况下…