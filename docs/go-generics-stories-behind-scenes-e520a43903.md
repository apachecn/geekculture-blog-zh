# 走向幕后的通用故事

> 原文：<https://medium.com/geekculture/go-generics-stories-behind-scenes-e520a43903?source=collection_archive---------11----------------------->

## Go 泛型实际上是由什么组成的

Go generics 已经基本定型。而且我已经谈过在类型匹配的两个主要特性中的基本用法，并且在我以前的文章 [*中约束 Go 泛型即将到来*](/geekculture/go-generics-is-coming-e03182d19873) *。*

但是，当我们能够深入了解时，我们决不会满足于一知半解。因此，我通读了一些提案文档，以抓住 Go 团队设计哲学的线索，并获得我脑海中问题的答案。例如，简单的`func Filter[T any](t` …