# PostgreSQL 连接策略概述

> 原文：<https://medium.com/geekculture/postgresql-join-strategies-overview-748c7ec88ea2?source=collection_archive---------9----------------------->

![](img/b143a8094503f2e072e2cd47e109c8b5.png)

在 Postgres 中，表之间的关系可以通过使用外键来表示。通过使用外键，可以使用 **JOIN** 子句在一个 PostgreSQL 查询中检索多个相关表中的数据。许多使用 Postgres 的开发人员熟悉所有类型的**连接**语法，但是只有一些人真正理解连接策略。连接策略是 Postgres 用来处理**连接**子句的内部算法。在这篇博文中，我将解释 Postgres 使用的三种连接策略。在进一步阅读之前，理解下面内容的先决条件是熟悉查询计划。如果你不是，请在这里阅读我的博客文章。

# 嵌套循环连接

嵌套循环连接是这三种算法中最简单的。让我们来看看这个查询。

```
EXPLAIN
  SELECT *
    FROM users
    JOIN orders
      ON orders.user_id = users.id
   WHERE users.id = 'HA100'
   LIMIT 1;
                     QUERY PLAN
--------------------------------------------------------------------------------------------------------------------
 Limit  (cost=1.14..21700.12 rows=1 width=200)
   ->  Nested Loop  (cost=1.14..58045700.05 rows=2675 width=200)
         ->  Index Scan using users_pkey on users  (cost=0.57..58022604.45 rows=2688 width=262)
               Filter: (id = 'HA100'::text)
         ->  Index Scan using orders_pkey on orders  (cost=0.57..8.58 rows=1 width=16)
               Index Cond: ((user_id)::text = (users.id)::text)
```

在查询计划中，您可以找出嵌套的循环节点。该过程可以翻译如下:

*   使用**索引扫描 *id* 列上的**找到**用户**表中的所有记录，因为 *id* 是主键。
*   对于之前从**用户**表中获得的每条记录，使用**订单**表中*用户 id* 列上的**索引扫描**遍历**订单**表，以找到满足条件`**orders.user_id = users.id**` 的记录。*用户标识*是外键。
*   然后，获得的结果被限制为 1 条记录并返回给用户。

如您所见，该算法使用 double for 循环来处理上例中的**连接**。一般来说，条件 A.ID = B.ID 的**连接**的嵌套连接的伪代码描述如下:

```
For each record r in relation A
  For each record s in relation B
    if (r.ID == s.ID)
      return r and s
```

在上面的例子中，关系 A 是带有`**id = 'HA100'**` 的**用户**表中的所有记录，关系 B 是**订单**表中的所有记录。如果语句为`**orders.user_id = users.id**`，则在**中检查的条件。**注意**:这个例子利用**索引扫描**来加速每次迭代的条件检查。**

# 散列连接

哈希连接算法需要分配额外的内存，因为它利用哈希表根据关系 A 和 B 条件查找记录。这里有一个例子。

```
SELECT e.name, e.address, s.amount
FROM employees e
INNER JOIN salary s 
  ON e.id = s.employee_id QUERY PLAN 
---------------------------------------------------
Hash Join (cost=35.42..297.73 …)
  Hash Cond: (e.id = s.employee_id)
    -> Seq Scan on employees e (cost=0.00..22.00)
    -> Hash (cost=21.30..21.30 rows=1130 width=8)
      -> Seq Scan on salary s (cost=0.00..21.30) 
(5 rows)
```

上面的查询处理如下:

*   对**工资**表进行顺序扫描。对于**薪资**表中的每条记录，根据*薪资 id* 计算一个哈希键。 *salary_id* 在这里用于哈希键，因为它在 **JOIN** 条件中使用。
*   将**薪资**表中的每条记录插入一个哈希表(显示在 **Hash** 节点)中，并计算哈希键。这是在顺序扫描中完成。
*   对**员工**表进行顺序扫描。对于**雇员**表中的每条记录，哈希键是基于 *id* 计算的，因为它被用于**连接**条件中。如果哈希表中存在计算出的哈希键，将返回哈希表桶中的**薪资**记录和**员工**记录。

散列连接的伪代码可以描述如下:

```
Relation A and B can be interchangeable.**Build phase:
**  * Iterate through relation B and calculate the hash key for each record. The hash key is based on the columns used in the JOIN condition. * In each iteration, insert record from relation B into a hash table with hash key calculated.**Probe phase: 
**  * Iterate through relation A and calculate the hash key for each record based on the value in columns used in the JOIN condition. * In each iteration, check if the hash key calculated is present in the hash table. If yes, return records in relation A and B.
```

在该示例中，**雇员**表是关系 A，而关系 B 是**薪水**表。

# 合并联接

合并联接要求来自 2 个关系的记录按照在**联接**条件中使用的列进行排序。为了理解我的意思，让我们看一下这个例子。

```
SELECT *
FROM employees e
JOIN salary s
  ON e.id = s.employee_id
WHERE e.age < 40;QUERY PLAN 
--------------------------------------------------------------
Merge Join  (cost=198.11..268.19 rows=10 width=488)
   Merge Cond: (e.id = s.employee_id)
   ->  Index Scan using employees_id on employees e  (cost=0.29..656.28 rows=101 width=244)
         Filter: (age < 40)
   ->  Sort  (cost=197.83..200.33 rows=1000 width=244)
         Sort Key: s.employee_id
         ->  Seq Scan on salary s (cost=0.00..148.00 rows=1000 width=244)
```

查询计划可以解释如下:

*   使用主键 *id* 上的**索引扫描**检索**雇员**表中**年龄**小于 40 的所有记录。**索引扫描**已经根据 *id* 值对记录进行了排序。
*   对**薪资**表进行顺序扫描，并根据*员工 id 对记录进行排序。*
*   然后，对获得的**员工**和**工资**记录进行并行扫描，寻找匹配记录。当 **e.id = s.employee_id** 时，匹配发生。

所提到的合并连接并行扫描以如下所述的一般术语工作。

```
Given cursor r and s pointing to top records of relation A and B respectively. Join condition is r.ID == s.IDFor (r != null and s!= null)
  If (r.ID == s.ID)
    Output r and s
  If (r.ID < s.ID)
    move r to next record
  Else 
    move s to next record
```

好了，伙计们。这是 Postgres 支持的三种连接策略。有一个练习给你:让我们思考一下这三个算法的时间和空间复杂度，如果你有答案，请在下面发表评论。

关注我，了解更多关于软件开发的内容。