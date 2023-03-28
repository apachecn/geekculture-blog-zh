# Python 函数和类的类型注释

> 原文：<https://medium.com/geekculture/python-type-annotation-for-functions-and-class-2e8e3148e376?source=collection_archive---------5----------------------->

## Python 类型注释深入研究

![](img/bf79d681408d23432700447f7439e197.png)

# 注释参数并返回值

要注释函数的类型，需要对每个形参进行类型注释，同时注释函数的返回值。例如:

```
def add(x: int, y: int) -> int:
    return x + y
```