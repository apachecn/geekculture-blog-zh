# SwiftUI: StateObject 和 ObservedObject

> 原文：<https://medium.com/geekculture/swiftui-stateobject-and-observedobject-c6640c1bd2fd?source=collection_archive---------1----------------------->

![](img/0b1263b1fed7dfcb1a5e46ba20831110.png)

Photo by [Aleksander Vlad](https://unsplash.com/@aleksowlade?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

嗨，

SwiftUI 是关于状态的。常见的属性包装器有`**State**`、`**EnvironmentObject**`、`**AppStorage**`、、`**StateObject**`、**、`**ObservedObject**`、**、**等。SwiftUI 中的视图是结构。结构没有被设计成可变异的。最初，一旦一个视图被渲染，它不会被改变。但是，在大多数情况下，视图包含动态数据。它们将被相应地更新。通过添加这些属性包装器，SwiftUI 创建了…**