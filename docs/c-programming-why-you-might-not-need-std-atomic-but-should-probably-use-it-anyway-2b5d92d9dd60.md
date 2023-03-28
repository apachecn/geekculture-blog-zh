# C++编程:为什么你可能不需要 std::atomic，但无论如何都应该使用它

> 原文：<https://medium.com/geekculture/c-programming-why-you-might-not-need-std-atomic-but-should-probably-use-it-anyway-2b5d92d9dd60?source=collection_archive---------20----------------------->

在花了数年时间在 Linux、Windows 以及 OS/2 上编写 C++多线程应用程序之后(我在 OS/2 1.0 上编写第一个多线程程序的时间比我愿意承认的要早)，我最近学到了一些让我吃惊的东西。它与在多线程应用中写入字节、字、双字或四字存储器地址(基本上是任何高达 64 位的高速缓存对齐值)时所需的数据保护类型有关。