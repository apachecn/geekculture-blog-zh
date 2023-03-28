# Golang 并发模式:扇入、扇出

> 原文：<https://medium.com/geekculture/golang-concurrency-patterns-fan-in-fan-out-1ee43c6830c4?source=collection_archive---------0----------------------->

## Go 开发模式

我个人喜欢 Golang 的一个最重要的原因是，我们可以很容易地构建一个高度可用的并发和非阻塞程序。

![](img/4b2e348e9eb848084d6e0b50b15173e1.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/lady-computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这一系列的文章中，我将试着回忆一下 Golang 中可用的模式。我将采用每种模式，并详细讨论它们适用于何处以及如何有效地使用它们。