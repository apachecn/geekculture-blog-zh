# Python 真值的隐藏陷阱

> 原文：<https://medium.com/geekculture/the-hidden-traps-of-python-truthy-values-51224e0413ab?source=collection_archive---------11----------------------->

以及如何避免它们。

![](img/c0264c4430192a191df76a8e1d16aa1d.png)

Photo by [John McArthur](https://unsplash.com/@snowjam?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

真值是在布尔上下文中使用时被认为是真的值。假值是在布尔上下文中使用时被视为假的值。这是 Python 和其他几种语言的一个有用特性。

它们对于编写简洁整洁的代码非常有用。但它们也隐藏着一个危险，会困住粗心的人。