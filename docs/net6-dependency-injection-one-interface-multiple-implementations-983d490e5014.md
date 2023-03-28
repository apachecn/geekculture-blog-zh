# . NET6 依赖注入—一个接口，多个实现

> 原文：<https://medium.com/geekculture/net6-dependency-injection-one-interface-multiple-implementations-983d490e5014?source=collection_archive---------0----------------------->

您希望用同一个接口实现多个实现。在这篇文章中，我将向您展示如何。

![](img/f7b2f8edd84b1404d0b56168a2a6d9e0.png)

Photo by [AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 使用泛型的多种实现

我有带 string Walk()方法的接口 IMovivement

```
public interface IMoviment<T> where T : class
```