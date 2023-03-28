# 线程软件的注意事项

> 原文：<https://medium.com/geekculture/considerations-for-threaded-software-4a656d5c7774?source=collection_archive---------37----------------------->

线程可能很容易，但没那么容易…

![](img/e554e74566d611421ee22eaf022c5642.png)

Photo by [John Anvik](https://unsplash.com/@redviking509?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大多数现代计算机都有多个处理器，或者至少有一个单处理器的快速上下文切换。即使你有一个处理器，也有很多时候它除了等待一个程序并为另一个程序服务之外无事可做。为了在你自己的程序中利用这一点，你可以使用线程。但是有几件棘手的事情关于…