# 在 SQL 中将一行扩展成多个相同行的 3 种方法

> 原文：<https://medium.com/geekculture/3-ways-to-expand-out-a-row-into-multiple-identical-rows-in-sql-53dcabc21c74?source=collection_archive---------6----------------------->

## 编写复杂 SQL 查询的小技巧

![](img/149f67e5af24848f956e8b8985bdaca9.png)

Photo by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将讨论如何基于特定的列将一行扩展成多个相同的行。例如，我们希望根据开始日期和结束日期之间的天数展开一行。或者我们希望根据数量创建相同的发票记录…