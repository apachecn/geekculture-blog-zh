# 如何在 TypeScript 中强类型化 try/catch 块

> 原文：<https://medium.com/geekculture/how-to-strongly-type-try-catch-blocks-in-typescript-4681aff406b9?source=collection_archive---------1----------------------->

![](img/c2c6dcc82b8090da60722e735879f179.png)

Oops

不幸的是，Javascript 不支持多个`**catch(error)**`来允许您基于错误类型运行不同的代码。但是，我们有办法在单个`**catch**`语句中改进错误处理，以利用类和`**instanceof**`操作符的力量。让我们看一个在 C#这样的语言中捕捉不同异常的例子。

## 不错的经典 C#…