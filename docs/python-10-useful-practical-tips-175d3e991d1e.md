# python——10 个有用的实用技巧

> 原文：<https://medium.com/geekculture/python-10-useful-practical-tips-175d3e991d1e?source=collection_archive---------2----------------------->

## 一些有用的 Python 技巧和窍门

![](img/1fdca98ce2895bb7852ceff936cbb299.png)

# 全部或任何

Python 语言如此受欢迎的众多原因之一是因为它非常易读和富于表现力。例如，下面的代码片段使用了`all`或`any`:

```
list_1 = [**True**, **True**, **False, False**]
**if** any(list_1):
    print("At least one True")
**if** all(list_1):
    print("All…
```