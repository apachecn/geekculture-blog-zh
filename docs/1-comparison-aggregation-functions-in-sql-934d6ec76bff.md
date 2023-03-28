# 1.SQL 中的比较和聚集函数

> 原文：<https://medium.com/geekculture/1-comparison-aggregation-functions-in-sql-934d6ec76bff?source=collection_archive---------8----------------------->

## 了解 SQL NULL 和聚合函数

## NVL、合并、解码、NULLIF、NVL2、AVG、求和、最小值、最大值、计数

![](img/2a8fe455af26276df6b3c72bd91fc7d4.png)

Photo by [path digital](https://unsplash.com/@pathdigital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/data-analytics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你好。！

我只是在修改 SQL 查询中使用的重要函数，同时为我的办公室工作处理 ETL 过程。所以我想，为什么不写一篇简短的文章或方便的笔记供将来参考。这些也是面试中被问到的一些重要的 SQL 问题。因此，在接下来的 3 篇文章中，我将写 SQL 查询中使用的 6 个主要函数类别。这些类别基本上是-

1- SQL 比较函数

2- SQL 聚合函数

3- SQL 字符串函数

4- SQL 日期函数

5- SQL 数学函数

6- SQL 窗口函数。

本文将仅限于前两个内置函数，即比较和聚合函数。此外，正如我提到的，这将是一种简短的笔记，所以我将保持简短，并将链接我发现最适合这些概念的外部材料。所以让我们一个一个来看，

**SQL 比较函数:**这些是单行函数(每行给出一个结果)，可用于任何数据类型(日期、字符和数字)，主要与数据库表中空值的使用有关。最常用的比较函数有 **NVL、联合、NULLIF、NVL2 &解码。**

1。 **NVL(expr1，expr2):** 将空值转换为实际值。在语法`**NVL(expr1, expr2),**` **中，expr1** 是源值或表达式**可能**包含空值， **expr2** 是目标值，如果在 expr1 中发现**空值，则转换/替换空值。语法是，**

```
**NVL(expr1,expr2)**
```

2。 **COALESCE():** 该函数返回列表中第一个**非空的**表达式。它首先从左到右检查第一个表达式，如果发现表达式不为空，则返回该表达式，否则它将进行评估，直到找到第一个**非空**参数。非空表达式之后剩余的所有表达式都不会被计算。

COALESCE()函数优于 NVL()函数，即 COALESCE 函数可以采用多个替代值/表达式。只有当列表中的所有表达式都为空时，才会返回空值。语法，

```
**COALESCE (expr1, expr2, expr3.............. exprN)**
```

3。 **NULLIF(expr1，expr2):** [这是一个控制流函数，它测试表达式并根据结果评估返回 NULL 或第一个表达式。它比较两个表达式，如果两个表达式相等，它将返回 NULL，如果不相等，它将返回第一个表达式。语法，](https://www.sqltutorial.org/sql-comparison-functions/sql-nullif/)

```
**NULLIF (expr1, expr2)**
```

4。 **NVL2(expr1，expr2，expr3)** :检查第一个表达式，如果 expr1(第一个表达式)不为空，则 NVL2 将返回 expr2(第二个表达式)，反之，如果 expr1 为空，则 NVL2 将返回 expr3。参数 expr1 可以有任何数据类型。语法，

```
**NVL2 (expr1, expr2, expr3)**
```

5。 **解码:** [该函数有助于以类似于 IF-THEN-ELSE 逻辑条件或 CASE 语句的方式解码表达式。此函数在将表达式与每个搜索值进行比较后对其进行解码。](https://www.sqltutorial.org/sql-comparison-functions/sql-decode/)

[如果表达式与搜索值相同，则返回结果。但是如果搜索值与任何结果值都不匹配，那么它将返回默认值，而如果没有提到默认值，它将返回 NULL。语法是，](https://www.sqltutorial.org/sql-comparison-functions/sql-decode/)

```
**DECODE(column|expression,** **search1**, **result1** , **search2**, **result2**,.......**searchN**, **resultN, [**, **default]**)
```

因此，这 5 个是最常用的空函数，现在我们将研究聚合函数。我们一个一个来看。

**SQL 聚合函数:**这些函数接受一组值并返回单个值。因为它对一组值进行操作，所以通常与 GROUP BY 子句一起使用。执行聚合函数的一般语法是:

```
**SELECT** c1, aggregate_fun (c2) 
**FROM** table
**GROUP** BY c1
```

最常用的聚合函数有****COUNT()****MAX()****MIN()****SUM()**。除了 Count()函数，所有其他函数都忽略空值。理解这些函数的工作是不言自明的，它只是关于理解业务需求和使用正确的语法。让我们简单地浏览一下这些函数。**

**1。AVG():返回集合中的平均值。寻找一组值的语法是`**AVG(All | Distinct)**`。默认情况下，选择 ALL，其中也包括重复值，而为了只查找不同值的平均值，我们使用关键字 distinct。显示从雇员表中计算每个部门平均工资的示例。**

```
**SELECT** department_name, **RROUND**(**AVG**(Salary),0) **AS** avg_salary
**FROM**
employees
**GROUP BY** department_name
**ORDER BY** department_name*Output:*
***department_name*      *avg_salary*** *Accounting           10800
Administration       20333
Executive            14500***Note: ROUND** Function have round-off the avg_salary to Zero decimal Places.
```

**2。 **COUNT():** 此函数返回集合中的项数。计算值的语法是`COUNT(* | DISTINCT)`。*用于计算所有值(也包括重复值)，而 Distinct 仅强制计算集合中存在的不同值。在 employee 表中查找各部门人数的示例:**

```
**SELECT** department_name, **COUNT**(*) **AS** num_employee
**FROM**
employees
**GROUP BY** department_name
**ORDER BY** department_name*Output:*
***department_name*      *num_employee*** *Accounting           5
Administration       2
Executive            6
Finance              5
IT                   7
Marketing            3
Sales                6*
```

**3。 **MAX():** 此函数返回集合中的最大值。从雇员表中查找每个部门雇员最高工资的示例。**

```
**SELECT** department_name, **MAX**(Salary) **AS** max_salary
**FROM** employees **GROUP BY** department_name **ORDER BY** department_name*Output:*
***department_name*      max*_salary*** *Accounting           17000
Administration       6000
Executive            5000
Finance              2500*
```

**4。 **MIN():** 与 Max 函数类似，MIN()函数用于返回一组值中的最小值/最低值。**

**5。 **SUM():** 返回集合中所有值的总和。语法是`SUM ( ALL |DISTINCT)`。在按部门分组的雇员表中查找所有雇员的工资总额，**

```
**SELECT** department_name, **SUM**(Salary) **AS** total_salary
**FROM** employees **GROUP BY** department_name **ORDER BY** department_name*Output:*
***department_name*      *total_salary*** *Accounting           59500
Administration       27800
Executive            57600
Finance              24600*
```

**这就是最常用的 SQL 聚合函数。**

**在下一篇文章中，我们将研究 SQL 字符串函数、SQL 日期函数和数学函数。**

**继续练习！！**

**感谢您的阅读！！**

**参考资料:**

 **[## SQL 教程-初学者必备的 SQL

### 本 SQL 教程通过许多实际例子帮助你快速有效地开始使用 SQL。如果你是一个…

www.sqltutorial.org](https://www.sqltutorial.org/)**  **[## SQL 教程- GeeksforGeeks

### 结构化查询语言或 SQL 是一种标准的数据库语言，用于创建、维护和检索数据

www.geeksforgeeks.org](https://www.geeksforgeeks.org/sql-tutorial/?ref=ghm)**