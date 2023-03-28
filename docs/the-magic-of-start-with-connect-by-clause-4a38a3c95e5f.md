# “从 Connect By 子句开始”的魔力

> 原文：<https://medium.com/geekculture/the-magic-of-start-with-connect-by-clause-4a38a3c95e5f?source=collection_archive---------5----------------------->

## 在 Oracle 数据库中导航层次结构

![](img/677424006012b648f575e8a0d541a1e1.png)

Photo by [NOAA](https://unsplash.com/@noaa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Oracle 数据库中，***【Start With Connect By】***子句是一种选择具有层次关系的数据的强大方法。(就像父母- >孩子或经理- >员工)。

我们将借助一个例子来探讨它。首先，让我们创建一个名为“实体”的表。