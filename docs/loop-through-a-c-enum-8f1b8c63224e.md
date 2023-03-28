# 循环通过 C#枚举

> 原文：<https://medium.com/geekculture/loop-through-a-c-enum-8f1b8c63224e?source=collection_archive---------4----------------------->

## 如何使用 C#

## C#枚举中的名称和值

![](img/e9b7375284a9adfad359d6094e0a2ec5.png)

如何**循环**遍历一个**C #**枚举中的所有条目？

**C#** 不提供与 **Java** 中的**枚举**相同的特性，但是仍然有一些东西需要去掉。

让我们在 **C#** 中定义一个简单的枚举:

```
public **enum** Colors
{
  red = 0,
  green = 1,
  blue = 2,
  yellow = 3
}
```