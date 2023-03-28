# Swift: willSet 和 didSet 简化了

> 原文：<https://medium.com/geekculture/swift-willset-and-didset-made-simple-a45d82536e9b?source=collection_archive---------9----------------------->

## 当值改变时执行代码。

![](img/2e453e442dd310940f34ce7114318381.png)

Photo by [Philipp Katzenberger](https://unsplash.com/@fantasyflip?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Swift，`willSet`和`didSet`被称为**物业观察员**。

属性观察器响应属性中的变化。例如，这段代码打印了一个分配给`word`的新字符串: