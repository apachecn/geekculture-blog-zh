# 你想知道的关于 Python 的 5 个技巧

> 原文：<https://medium.com/geekculture/5-tricks-about-python-you-wish-knew-about-1e371eb790cd?source=collection_archive---------13----------------------->

## 我希望我以前就知道这些…

![](img/49cc559e9351ca8aa0df283921e9389e.png)

Photo by [**Olya Prutskova**](https://www.pexels.com/@olya-prutskova-35454617?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/man-leaning-on-wall-with-birds-shadows-from-projector-7163732/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 展平列表列表

**问题:**如何将`**[[1, 2, 3], [4, 5, 6], [7], [8, 9]]**` **或** `**[[1, 2, 3], [4, 5, 6], [7, 8, 9]]**`转换成`**[1, 2, 3, 4, 5, 6, 7, 8, 9]**`？

**答案:**最简单的答案是使用一个空列表和 for 循环，就像这样(在这里，我们将引用…