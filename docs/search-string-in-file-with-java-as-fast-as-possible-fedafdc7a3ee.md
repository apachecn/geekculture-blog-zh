# Java —尽可能快地在文件中搜索

> 原文：<https://medium.com/geekculture/search-string-in-file-with-java-as-fast-as-possible-fedafdc7a3ee?source=collection_archive---------2----------------------->

## 溪流？平行流？无功通量？正则表达式？有很多不同的方法。让我们看看什么是最好的解决方案。

![](img/36bbe64444420bfbbd94016c4201aa66.png)

Photo by [Jake Givens](https://unsplash.com/@jakegivens?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

环境:大约 1GB 的文本文件，大约 600 万行。

让我们从最简单的开始；标准的“读取内存中的所有文件并在那里搜索”。的…