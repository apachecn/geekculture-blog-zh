# sklearn 的计数矢量器和 TfidfTransformer 与 TfidfVectorizer 相比如何

> 原文：<https://medium.com/geekculture/how-sklearns-countvectorizer-and-tfidftransformer-compares-with-tfidfvectorizer-a42a2d6d15a2?source=collection_archive---------5----------------------->

![](img/903f6fa4cd79b69a07283d003e883345.png)

在我最近的一篇文章中，我讨论了 sklearn 的 CountVectorizer 及其使用方法，它基本上是统计语料库中单词的出现次数。在以前的文章中，我讨论了 TfidfVectorizer，它对 corpos 进行矢量化，并准备将其输入到估计器中。有些人没有意识到的事实是 TfidfVectorizer 是一个函数…