# 选择数据工程

> 原文：<https://medium.com/geekculture/go-for-data-engineering-a45b6f858a11?source=collection_archive---------2----------------------->

最近，有人问我一些有趣的问题，关于 [Go(又名 Golang)](https://go.dev/) 是否比 [Python](https://www.python.org/) 执行得更快。这是一个哲学问题，使用特定的编程语言进行开发取决于用例场景。我意识到这么多人开始使用 [Go](https://go.dev/) 进行[数据工程](https://www.coursera.org/articles/what-does-a-data-engineer-do-and-how-do-i-become-one)来实现更高的性能。因此，我正在制作一个经典的例子来演示和比较用于[数据工程](https://www.coursera.org/articles/what-does-a-data-engineer-do-and-how-do-i-become-one)任务的 [Go](https://go.dev/) 和 [Python](https://www.python.org/) 语言的性能。这个演示将把 10，000 个 JSON 文件解析成一个 CSV 文件。