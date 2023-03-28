# Python 中的责任链模式

> 原文：<https://medium.com/geekculture/chain-of-responsibility-pattern-in-python-9662680b8567?source=collection_archive---------11----------------------->

解耦复杂的决策链。

![](img/f53fba3df7e9e5cf1aa72ee5b90b2725.png)

Photo by [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

责任链是一种行为设计模式。它用于处理*命令对象*，不同类型的命令对象可能需要以不同的方式处理。

它通过允许多个*处理程序对象*来做到这一点，每个对象能够处理特定类型的命令并拒绝其他命令。命令…