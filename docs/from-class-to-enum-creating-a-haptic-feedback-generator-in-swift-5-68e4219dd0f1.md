# 从类到枚举:在 Swift 5 中创建触觉反馈生成器

> 原文：<https://medium.com/geekculture/from-class-to-enum-creating-a-haptic-feedback-generator-in-swift-5-68e4219dd0f1?source=collection_archive---------38----------------------->

## 从冗长到简洁

## 从重到轻

![](img/2347db784e9d9a79ac2dd5295170f579.png)

Photo by [Elia Pellegrini](https://unsplash.com/@eliapelle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/touch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

假设您正在创建一个 singleton 或[静态类](#7ed7)来表示永远不会改变的数据和/或逻辑。在许多情况下，您可以将这种逻辑转换成枚举，这样可以省去很多麻烦。枚举具有类的大部分功能，但是…