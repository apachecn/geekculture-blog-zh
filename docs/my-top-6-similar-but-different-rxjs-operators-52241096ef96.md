# 我的前 6 名“相似但不同”的 RxJS 操作员

> 原文：<https://medium.com/geekculture/my-top-6-similar-but-different-rxjs-operators-52241096ef96?source=collection_archive---------6----------------------->

分享我的六大“相似但不同”的 RxJS 操作员

![](img/d48ff49faeddd0a7da5abed9a1808ab0.png)

# 介绍

RxJS 提供了许多运算符来处理数据。毫无疑问，这是一件好事，但是，在特定情况下选择正确的操作符可能会令人困惑。因此，如果您不熟悉 RxJS，这可能会导致您使用错误的操作符。

在这篇文章中，我将分享我的 6 个常见的“相似但不同”的 RxJS 运算符，通过避免它们来帮助你节省时间。

# 单()，取(1)和第()

**first():** 只发出源可观察对象发出的第一个值(或满足某些条件的第一个值)，如果上游为空，则发出一个错误。

**take(1):** 将上游项目的数量限制为 1，如果上游为空，则不会发出错误。

**single():** 确保上游不为空。

总而言之，我们可以说`first == take(1).single()`其中`take(1)`将上游项目的数量限制为 1，`single()`确保上游不为空。

# map()与 switchAll()与 switchMap()

**map():** 对每个值应用一个投影，并在输出可观测值中发出该投影。

**switchAll():** 取一个**高阶可观测值**，订阅最近提供的内部可观测值，退订之前订阅的。

**switchMap():** 是`map()`和`switchAll()`的组合。

# zip()vs combineLatest()vs fork join()

**zip():** 取一组可观测量，等到所有可观测量都发出一个值，然后将所有可观测量作为一个数组发出。

**combineLatest():** 和`zip()`一样，`combineLatest()`会一直等待，直到所有的观察值都以数组的形式发出一个值。从那里，每当一个可观察值发出一个值时，它将发出一个数组。

**forkJoin():** 取一组可观测量，等待它们完成发射，将每个可观测量的最终值作为数组发射出去。

# merge() vs mergeAll() vs mergeMap()

**merge():**

**mergeAll():** 将一组**高阶可观测量**发出的内部可观测量合并，展平为一个一阶输出可观测量。

**mergeMap():** 是`map()`和`mergeAll()`的组合。

# concat()vs concat all()vs concat map()

与`merge()`、`mergeAll()`和`mergeMap()`相同，除了 concat 的可观察对象必须在下一个开始发送值之前完成。

# 窗口()与缓冲区()

**buffer():** 缓存可观察对象发出的值，并在`notifier`发出时将它们作为数组发出。

**window():** 当`windowBoundaries`发出时，将一个可观测项细分为嵌套的可观测项。

# 结论

最后，Rx 是一个巨大的库，可以帮助您处理应用程序复杂的异步方面。在这篇文章中，我试图阐明常用的运算符，并根据相似性对它们进行分组。