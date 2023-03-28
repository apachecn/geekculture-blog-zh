# C#中如何连接对象？

> 原文：<https://medium.com/geekculture/how-to-concatenate-objects-in-c-46f2ce7f00a4?source=collection_archive---------6----------------------->

## 不仅仅是弦乐。

![](img/6dd4d95da8049b75c454ce7b5f670988.png)

Photo by [Blake Connally](https://unsplash.com/@blakeconnally?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

字符串连接是一个非常简单的操作，它允许您将多个字符串连接在一起。通常的做法是使用 **'+'** 运算符:

```
string greeting = "Hello, " + "World!";
```

此外，还有其他几种连接字符串的方法:使用`string`类的`Join`、`Concat`、`Format`方法或使用…