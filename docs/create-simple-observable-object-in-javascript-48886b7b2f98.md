# 用 JavaScript 创建简单的可观察对象

> 原文：<https://medium.com/geekculture/create-simple-observable-object-in-javascript-48886b7b2f98?source=collection_archive---------19----------------------->

![](img/25091cffc6bbe759aaeab8bfe28ba75c.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用 JavaScript 可能会很棘手。如果你不知道基本的核心特性，你最终会在你的项目中添加大量的库。如果你想创建一个简单的可观察对象，当你更新任何属性时。它会触发订阅者。使用代理可以很容易地实现这一点。

首先，你需要一个代理类。你必须创建一个代理对象来清空…