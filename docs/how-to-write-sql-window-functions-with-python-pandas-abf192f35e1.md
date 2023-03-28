# 如何用 Python 熊猫写 SQL 窗口函数

> 原文：<https://medium.com/geekculture/how-to-write-sql-window-functions-with-python-pandas-abf192f35e1?source=collection_archive---------3----------------------->

举例说明。

![](img/6dbff1214609136a1dec2e43fa7dd25d.png)

Photo by [Elena Mozhvilo](https://unsplash.com/@miracleday?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/similarity?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

SQL 窗口函数是很好的数据分析工具。它们主要做的是对一组有某种关联的行执行计算。

假设我们有一个包含雇员列表及其部门和薪水的表。为了计算员工的平均工资，我们可以只取…