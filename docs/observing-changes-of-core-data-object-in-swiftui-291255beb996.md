# 观察 swiftUI 中核心数据对象的变化

> 原文：<https://medium.com/geekculture/observing-changes-of-core-data-object-in-swiftui-291255beb996?source=collection_archive---------0----------------------->

![](img/437d8fc773ab662dffa82f8447e35f7d.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

核心数据对象符合可观察性，每个属性都是可发布的。所以我们可以使用“@observedObject”来观察核心数据对象的变化。让我们看一个小例子。一个双屏幕应用程序，其中一个屏幕有一个核心数据对象列表，另一个屏幕提供了编辑核心数据对象属性的选项。 **By** **使用下面的' @ObservedObject '，我们对核心数据对象所做的更改会自动反映出来**。

感谢阅读:)