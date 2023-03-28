# 理解开放递归和继承

> 原文：<https://medium.com/geekculture/understanding-open-recursion-and-inheritance-1ff4e3277b0a?source=collection_archive---------10----------------------->

# 摘要

*   继承可以用来重写方法。
*   例如，这可以用来记忆递归函数。
*   动态分派可以归结为连续传递:一个功能概念。
*   这种形式的延续传递可以被称为“开放递归”，因为递归调用是开放的，可以被覆盖。

# 使用继承的记忆化