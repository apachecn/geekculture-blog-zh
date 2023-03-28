# 限制 Go 中的无限制并发性(第 3 部分)

> 原文：<https://medium.com/geekculture/limit-unbound-concurrency-in-go-part-3-3161791d1599?source=collection_archive---------6----------------------->

## 在 Go 中实现一个端口扫描器，并解释扇入和扇出模式

一个**端口扫描器**被设计用来探测一个[服务器](https://en.wikipedia.org/wiki/Server_(computing))或[主机](https://en.wikipedia.org/wiki/Host_(network))是否打开[端口](https://en.wikipedia.org/wiki/TCP_and_UDP_port)。在这一系列文章中，我们将在 Go 中实现一个端口扫描器，同时解释一些并发概念。

我们将在限制无限制并发性的第 3 部分中讨论扇入和扇出模式。如果您错过了 [**第一部分**](https://levelup.gitconnected.com/limit-unbound-concurrency-in-go-part-1-72f7cedf2e61?source=your_stories_page----------------------------------------) **和** [**第二部分**](https://jerryan.medium.com/limit-unbound-concurrency-in-go-part-2-a00ada3bb50f) ，请阅读它们以了解必要的背景知识。