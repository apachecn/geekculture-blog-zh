# 优化 SQL 查询的 9 种方法

> 原文：<https://medium.com/geekculture/9-ways-to-optimize-sql-queries-f62680d6f59a?source=collection_archive---------0----------------------->

增强您的 SQL 查询

SQL 查询优化很重要，就像数据库管理系统的任何其他组件一样。如果不优化访问数据的查询，数据库的性能会受到影响。在许多情况下，这种速度减慢会阻止用户快速访问必要的信息。本文将讨论各种 SQL 查询优化技术，这些技术可用于提高查询性能并降低解决方案的成本。

![](img/500107065f1422b08155438ca43ac16f.png)

Photo by [Hiroshi Kimura](https://unsplash.com/@muukii) on [Unsplash](https://unsplash.com/photos/rtX4wxMEI2M)

**提示 1。在 select 语句中不要使用*而是使用列名**

如果您只想选择一定数量的列，那么在 select 语句中应该使用列名而不是*。尽管这更容易编写，但是数据库需要更多的时间来处理查询。通过限制选择的列数，可以减小结果表的大小，降低网络流量，并从整体上提高查询性能。

示例:

```
**Original Query :**Select * from sales;**Improved Query :**Select product_id from sales;
```

**提示二。而不是使用 WHERE 来定义过滤器**

SQL 优化查询将只从数据库中检索必要的记录。HAVING 语句是根据 SQL 操作顺序在 WHERE 语句之后计算的。如果目标是根据条件筛选查询，WHERE 语句会更有效。

示例:

```
**Original query:**SELECT customer_id,count(customer_id)FROM salesGROUP BY customer_idHAVING customer_id != '16' AND customer_id != '2';**Improved query:**SELECT customer_id,count(customer_id)FROM salesWHERE customer_id != '16'AND customer_id !='2'GROUP BY customer_id;
```

**提示 3。避免不必要的不同情况**

使用 Distinct 语句是删除重复项的简便方法。它通过在查询中创建组来工作。但是，要实现这个目标，需要大量的计算能力。此外，数据可能在一定程度上被不准确地分类。解决方案是选择更多的字段来产生不同的结果，而不是使用 SELECT DISTINCT。

示例:

```
**Original Query:**SELECT DISTINCT FirstName, LastName, StateFROM Teachers;**Improved Query**SELECT FirstName, LastName, Address, State,CourseName,TimingsFROM Teachers;
```

**提示 4。使用连接代替子查询**

使用 join 的优点是，与子查询相比，它的执行速度更快。与执行所有查询并加载所有数据以执行处理的子查询不同，联接允许 RDBMS 构建更适合您的查询的执行计划，并可以预测应该加载哪些数据以进行处理，从而节省时间。

示例:

```
**Original query:**SELECT *FROM products pWHERE p.product_id =(SELECT s.product_idFROM sales sWHERE s.customer_id = 2468AND s.quantity_sold = 12 );**Improved query:**SELECT p.*FROM products p, sales sWHERE p.product_id = s.product_idAND s.customer_id = 2468AND s.quantity_sold = 12;
```

**提示 5。查询索引列时使用 In 谓词**

对于索引检索，可以使用 IN-list 谓词，优化器可以对 IN-list 进行排序，以匹配索引的排序顺序，从而实现更有效的检索。请记住，IN-list 只能包含常量，也就是说，在查询块的单次执行过程中保持不变的内容，比如外部引用。

示例:

```
**Original query:**SELECT *FROM salesWHERE product_id = 4OR product_id = 7;**Improved query:**SELECT *FROM salesWHERE product_id IN (4, 7);
```

**提示 6。当使用涉及具有一对多关系的表的表联接时，use existing 而不是 DISTINCT。**

DISTINCT 通过在查询中创建组来工作，这需要大量的计算能力。可以将子查询与 EXISTS 关键字一起使用，这样可以避免返回整个表。

示例:

```
**Original query:**SELECT DISTINCT c.country_id, c.country_nameFROM countries c, customers eWHERE e.country_id = c.country_id;**Improved query:**SELECT c.country_id, c.country_nameFROM countries cWHERE EXISTS (SELECT * FROM customers eWHERE e.country_id = c.country_id);
```

**提示 7。尽可能使用 Union ALL 而不是 Union**

Union ALL 的执行速度比 Union 快，因为在 Union 中，重复项无论是否存在都会被删除。Union ALL 显示重复的数据。

示例:

```
**Original query:**SELECT customer_idFROM salesUNIONSELECT customer_idFROM customers;**Improved query:**SELECT customer_idFROM salesUNION ALLSELECT customer_idFROM customers;
```

**提示 8。避免在连接查询中使用 OR**

如果在联接查询时使用或，查询速度会降低 2 倍。

示例:

```
**Original query:**SELECT *FROM costs cINNER JOIN products p ON c.unit_price =p.product_min_price OR c.unit_price = p.product_list_price;**Improved query:**SELECT *FROM costs cINNER JOIN products p ON c.unit_price =p.product_min_priceUNION ALLSELECT *FROM costs cINNER JOIN products p ON c.unit_price =p.product_list_price;
```

**提示 9。避免在运算符右侧使用聚合函数**

避免在运算符的右侧使用聚合函数将极大地优化 SQL 查询。

示例:

```
**Original query:**SELECT *FROM salesWHERE EXTRACT (YEAR FROM TO_DATE (time_id, ‘DD-MON-YYYY’)) = 2021 AND EXTRACT (MONTH FROMTO_DATE (time_id, ‘DD-MON-YYYY’)) = 2002;**Improved query:**SELECT * FROM salesWHERE TRUNC (time_id) BETWEENTRUNC(TO_DATE(‘12/01/2001’, ’mm/dd/yyyy’)) ANDTRUNC (TO_DATE (‘12/30/2001’,’mm/dd/yyyy’));
```

**结论**

查询优化是由数据库管理员、数据分析师和应用程序设计人员执行的常规操作，用于微调数据库系统的整体性能。因此，遵循这些简单的提示将有助于优化 sql 查询。

参考来自 Jean Habimana 和 Analytics vidhya 关于查询优化技术的研究论文——查询优化技术。

Diksha Mohnani 是一名商业分析师、作家、舞蹈家和创业爱好者。她目前在沃尔玛担任商业分析师。她热衷于将技术知识与她的创造力和领导技能结合起来，打造出优秀的产品。