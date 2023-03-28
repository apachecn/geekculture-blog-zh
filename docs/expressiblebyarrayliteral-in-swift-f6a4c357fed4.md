# Swift 中的 ExpressibleByArrayLiteral

> 原文：<https://medium.com/geekculture/expressiblebyarrayliteral-in-swift-f6a4c357fed4?source=collection_archive---------13----------------------->

![](img/4633a24e9585dbfb7a569c9d077ae1f9.png)

Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当使用 **struct** s 时，您可能会在许多情况下遇到**。乍一看，这似乎很难理解，但是了解它实际上是如何工作的以及它提供了什么是很有帮助的。**

**expressablebyarrayliteral**，顾名思义，是一个**协议**，允许使用数组文字初始化它所符合的类型。此外，它可以增加可读性。为了清楚起见，您将把它作为一个简单的例子来研究。

比方说，你有一个类型为 **struct** 的**堆栈**。

```
**struct** Stack<Element: Equatable>: Equatable {
  /// Holds the stack data.
  **private** **var** storage: [Element] = []

  **init**() { }
}
```

当你初始化**堆栈**时，你必须这样初始化:

```
**let** stack = Stack(["expressible", "by", "array", "literal"]
```

但是，如果您将**ExpressibleByArrayLiteral**扩展添加到**堆栈**中:

```
**extension** Stack: ExpressibleByArrayLiteral {
  **init**(arrayLiteral elements: Element…) {
    storage = elements
  }
}
```

您可以像这样初始化**堆栈**:

```
**let** stack: Stack = ["expressible", "by", "array", "literal"]
```

实现就是这么简单！现在，您可以在自己的项目中试用它。你可以在这里 访问我用这个 [**的 GitHub 库。**](https://github.com/yusasarisoy/Stack)

感谢阅读！