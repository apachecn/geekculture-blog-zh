# 优化拥有数百万数据的 PostgreSQL 数据库的 4 种方法

> 原文：<https://medium.com/geekculture/4-ways-to-optimise-postgresql-database-with-millions-of-data-c70e11d27a94?source=collection_archive---------1----------------------->

本系列包括两篇文章:

1.  [如何在几秒钟内用 PostgreSQL 创建 3750 万条数据](/geekculture/how-to-create-37-5-million-data-in-postgresql-in-a-matter-of-seconds-858693976d17?sk=888ccfc6d0747b5df266955497ce7e7a)
2.  [优化拥有数百万数据的 PostgreSQL 数据库的 4 种方法](https://josipvojak.com/4-ways-to-optimise-postgresql-database-with-millions-of-data-c70e11d27a94?sk=d2c6b400b64e89304a0a2da28eb83531)

![](img/2c8177a9bebc93e3abbd7dd9f2b162b5.png)

数据库优化实际上是一套技术，我们通常希望通过这些技术实现以下一些目标:

*   **加速数据库操作**
*   **减少负载**
*   **减小数据库的大小**
*   **利用开箱即用的特性来帮助整体数据库优化**

在上一篇文章中，我们使用 **generate_series()** 函数生成了真实数据——这有助于在短时间内创建大量的测试数据。

如果你有兴趣阅读，这里有一个链接:

[](/geekculture/how-to-create-37-5-million-data-in-postgresql-in-a-matter-of-seconds-858693976d17) [## 如何在几秒钟内用 PostgreSQL 创建 3750 万个数据

### 使用这种简单有效的技术创建大量预定义的数据。

medium.com](/geekculture/how-to-create-37-5-million-data-in-postgresql-in-a-matter-of-seconds-858693976d17) 

对于那些读过的人，让我提醒你们——对于那些没读过的人，让我向你们介绍发生了什么:

我们模仿矿工的行为来制造一种虚构的加密货币——我们有几个矿工，根据显卡的数量不同而不同:

我们创建了三个表:

*   **小时**
*   **矿工**
*   **矿工 _ 数据**

***小时*** 表包含一天中特定时间计算机冷却器的强度
***矿工*** 表包含基本数据，如图形卡的名称和数量:

```
+-----+------------+----------------+
| id  |    name    |  graphic_cards |
+-----+------------+----------------+
|  1  |  Diamond   |             10 |
|  2  |  Platinum  |              7 |
|  3  |  Gold      |              4 |
|  4  |  Silver    |              3 |
|  5  |  Bronze    |              2 |
|  6  |  Default   |              1 |
+-----+------------+----------------+
```

***miner_data*** 表包含了一年(*2020 年 10 月 14 日—2021 年 10 月 14 日*)期间，计算机风扇在这些天的行为的总计 3750 万个数据:

```
// ascending (first entry)
**select * from miner_data order by time asc limit 1;** time          | miner_name |  fan_percentage   
------------------------+------------+-------------------
 2020-10-14 00:00:00+02 | Silver     | 80.02813264708547
(1 row)
Time: 10228.051 ms (00:10.228)// descending (last entry)
**select * from miner_data order by time desc limit 1;** time          | miner_name |  fan_percentage
------------------------+------------+------------------
 2021-10-14 00:00:00+02 | Platinum   | 74.8233384374659
(1 row)
```

数据以 5 分钟的时间间隔生成。

**正如您在上面看到的，像 SELECT 这样的简单查询，数据按时间排序，并且设置了 1 或 10 条记录的限制，持续时间长达 10-12 秒。**

## 解释与解释分析

PostgreSQL 提供了两个有趣的命令— **解释**和**解释**分析。

不同之处在于，EXPLAIN 根据收集的数据库统计数据显示查询成本，而 EXPLAIN ANALYZE 实际上运行它来显示每个阶段的处理时间。

强烈建议使用 EXPLAIN ANALYZE，因为在很多情况下，EXPLAIN 显示出较高的查询成本，而执行时间实际上较少，反之亦然。最重要的是，EXPLAIN 命令将帮助您理解是否使用了特定的索引以及如何使用。查看索引的能力是学习 PostgreSQL 查询优化的第一步。

以下是上述查询示例的结果:

EXPLAIN vs EXPLAIN ANALYZE

现在，让我们看看可以提高数据库性能的四个简单步骤。

# 1.数据库索引

数据库索引**是一种数据结构，它以额外的写入和存储空间**为代价来提高数据库表**上的数据检索操作**的速度，以维护索引数据结构。

如何在创建表时看到哪些索引是 PostgreSQL 自动设置的？

```
SELECT
  tablename,
  indexname,
  indexdef
FROM
  pg_indexes
WHERE
  schemaname = ‘miner_data’
ORDER BY
  tablename,
  indexname;
```

让我们尝试在 time 和 miner name 列上创建一个简单的索引:

```
**CREATE INDEX ON miner_data(“time”, “miner_name”);**SELECT * FROM miner_data LIMIT 1;
+-------------------------+--------------+--------------------+
|     time                |  miner_name  |    fan_percentage  |
+-------------------------+--------------+--------------------+
| 2020-10-14 00:00:00+02  |  Silver      |  80.02813264708547 |
+-------------------------+--------------+--------------------+(1 row)
Time: 0.469 ms
```

**预优化:12004.737 毫秒**

**优化后:0.469 毫秒**

正如我们所看到的，最简单的索引添加使我们获得了巨大的提升**25596 倍！**

因此，如果您正在处理大量数据，索引是您需要注意的事情。

# 2.查询优化

比方说，我们想要检索一段时间内每个矿工的计算机冷却器的最大值。

查询可能如下所示:

```
**SELECT 
 miner_name, 
  MAX(fan_percentage) 
FROM miner_data 
WHERE miner_name IN 
  (SELECT DISTINCT "name" 
   FROM miners) 
GROUP BY 1 
ORDER BY 1;** miner_name |        max        
------------+-------------------
 Bronze     |  94.9998652175735
 Default    | 94.99994839358486
 Diamond    | 94.99999006095052
 Gold       |  94.9998083591985
 Platinum   | 94.99982552531682
 Silver     | 94.99996029210493Time: 9173.750 ms (00:09.174)
```

不考虑设置索引，这个操作开销很大。

我们花了**9 秒多的时间**才完成。假设您的网站的一个功能是允许用户查看这样一组数据，并且页面加载时间至少为 9 秒(不考虑其他数据和查询、额外的数据处理、延迟等)。).

谁会愿意等 10 秒钟才得到数据呢？

例如，我们在这里可以做的是以不同的方式重写查询，以减少所需操作的数量、查看和比较的行数，从而加快查询速度:

```
**SELECT 
  DISTINCT ON (miner_name) miner_name, 
  MAX(fan_percentage) 
FROM miner_data 
GROUP BY miner_name;**miner_name |        max        
------------+-------------------
 Bronze     |  94.9998652175735
 Default    | 94.99994839358486
 Diamond    | 94.99999006095052
 Gold       |  94.9998083591985
 Platinum   | 94.99982552531682
 Silver     | 94.99996029210493Time: 2794.690 ms (00:02.795)
```

通过以更智能的方式编写查询，我们节省了时间。

**预优化:9173.750 毫秒**

**优化后:2794.690 毫秒**

对于这种情况，通过以更好的方式编写查询，**我们将处理速度提高了 3.28 倍。**

# 3.创建实体化视图

物化视图**是从查询规范**(视图定义中的选择)导出的预先计算的数据集，并被存储以备后用。因为数据是预先计算的，所以查询实例化视图比对视图的基表执行查询要快。

如果我们使用上面的查询:

```
SELECT 
 DISTINCT ON (miner_name) miner_name,
 MAX(fan_percentage) 
FROM miner_data 
GROUP BY miner_name;
```

并使其成为物化视图，命名为**max _ fan _ percentage _ by _ miner**:

```
CREATE MATERIALIZED VIEW max_fan_percentage_by_miner AS 
SELECT 
 DISTINCT ON (miner_name) miner_name,
 MAX(fan_percentage) FROM miner_data
GROUP BY miner_name;
```

让我们看看使用物化视图检索每个矿工的计算机冷却器的最大值的时间:

```
**SELECT * FROM max_fan_percentage_by_miner;** miner_name |        max        
------------+-------------------
 Bronze     |  94.9998652175735
 Default    | 94.99994839358486
 Diamond    | 94.99999006095052
 Gold       |  94.9998083591985
 Platinum   | 94.99982552531682
 Silver     | 94.99996029210493Time: 0.247 ms
```

**预优化:2794.690 毫秒**

**优化后:0.247 毫秒**

数据检索提高了 11，314.53 倍。

# 4.规范化表(使用外键)

这是一个非常基本的问题，但是需要注意。从附件中可以看出，虽然有一个 ***矿工*** 表，其中包含矿工 **id** (这是一个整数)，矿工名称和其他一些东西，但在**矿工 _ 数据**表中，我们使用矿工名称，而不是其 id。

我们可以规范化该表，使用外键作为与矿工表的关系(列" **id"** )。

**INT (integer)比较比 VARCHAR 比较快，原因很简单，INT 比 VARCHAR 占用的空间少得多。**

这适用于无索引和有索引的访问。最快的方法是索引 INT 列。

# 结论

虽然这些都是一些基本的优化技术，他们可以结出非常大的果实。此外，尽管这些技术很简单，但要做到以下几点并不容易:

*   知道如何**优化查询**
*   创建足够有效数量的**数据库索引**,而不在磁盘上创建大量数据——从而可能产生反效果，鼓励数据库以错误的方式进行搜索
*   知道什么时候使用**视图**更好，什么时候使用**物化视图**

你需要处理数据，直到你找到一个合适的公式来适应你的模型。