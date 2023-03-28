# 使用 Python LRU 缓存装饰器

> 原文：<https://medium.com/geekculture/python-lru-cache-2680be8045aa?source=collection_archive---------14----------------------->

![](img/362e4c54c0d4b2fa33251a99caaa7baf.png)

hoto by [Vishnu Mohanan](https://unsplash.com/@vishnumaiea?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 亲近你的朋友

许多函数(实际上是所有不依赖于外部输入和随机性的函数)在用相同的参数调用时，预计会返回相同的值。比如，`math.factorial(10)`永远是 3_628_800，`math.sin(math.pi)`永远是 1.2246467991473532e-16，至少对于浮点库的同一个实现是这样的。此外，这些功能…