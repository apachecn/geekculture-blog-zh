# Postgres BRIN 索引—以最少的存储实现大数据性能

> 原文：<https://medium.com/geekculture/postgres-brin-index-large-data-performance-with-minimal-storage-4db6b9f64ca4?source=collection_archive---------1----------------------->

这个故事重点讲述了一种特定的索引类型**布林** *(块范围索引)*它的用途，它比其他索引类型更高效的使用案例。此外，我们还绘制了与 Postgres 中其他流行的索引类型相比较的性能指标。

![](img/5d0e8bf8c9e59fe6bd9b29b3b3dabaf3.png)

**什么是布林指数？**

布林是一个区间指数。块是 Postgres 的基本存储单元，默认情况下是 8kB 的数据。BRIN 对某个范围的块进行采样(默认值为 128)，存储该范围内第一个块的位置以及这些块中所有值的最小值和最大值。它对于有序数据集非常有用，为类似甚至更好的性能节省了大量空间。

BRIN Index 是 PostgreSQL 贡献者 Alvaro Herrera 首先提出的一个革命性的索引概念[。布林代表“区块范围指数”。块范围是一组彼此相邻的页面，所有这些页面的摘要信息都存储在索引中。例如，像整数(排序顺序为线性的日期)这样的数据类型可以存储为该范围内的最小值和最大值。包括 Oracle 在内的其他数据库系统后来也宣布了类似的功能。BRIN 索引通常可以获得与对表进行分区类似的收益。](https://www.postgresql.org/message-id/20130614222805.GZ5491@eldon.alvh.no-ip.org)

布林是一个轻量级指数，经常被误解。如果实施正确，它会带来显著的好处，如节省空间和速度。然而，如果实施不当，就会失去一些好处。

BRIN 在高效搜索大型时间序列数据方面非常有用，并且比标准 B 树索引占用的磁盘空间少得多。块范围索引条目指向一个页面(PostgreSQL 存储数据的原子单位)并存储两个值:页面的最小值和要索引的项的最大值。

**它能用在什么地方？**

如今，许多应用程序记录来自传感器、设备、跟踪信息、实时银行交易和其他事物的数据，这些数据都有一个共同的属性:一个不断增加的时间戳。这个时间戳非常有价值，因为它是各种查找、分析查询等的基础。

如果使用得当，布林索引不仅会优于 B 树，还会节省超过 99%的磁盘空间。

现在让我们建立一个示例团体售票系统。售票系统将有一个唯一的 id、该票的人数、以及该票被带来时的时间戳。

```
**CREATE** **TABLE** ticketing_system (
    ***id*** BIGSERIAL,
    ***ticket_id*** UUID not null default uuid_generate_v4(),
    ***count*** int,
    ***created_at*** timestamptz NOT NULL);
```

现在让我们插入一些随机数据，这在本质上是递增的。，增加时间戳。我们将以 5 秒的间隔插入从 2015 年到当前日期 2021 年 4 月 23 日的数据。

```
**INSERT** **INTO** *ticketing_system* *(count, created_at)*
**SELECT** **floor**(**random**() ** 10 + 1*)::**int**, *dt*
**FROM** **generate_series**(*'2015-01-01 0:00'::***timestamptz**,
*'2021-04-23 23:59:50'*::**timestamptz**, *'5 seconds'::***interval**) *dt*;
```

> 上面的 insert 语句将根据创建的日期范围在表上插入大约 3900 万行。

让我们查询这个表，返回一个月中每天的平均人数。

```
**SELECT** **date_trunc***('day', created_at)*, **avg**(**count**) **FROM** *ticketing_system ts* **WHERE** *created_at* **BETWEEN** *'2021-02-01 0:00'* **AND** *'2021-02-28 11:59:59'* **GROUP** **BY** *1* **ORDER** **BY** *1*;
```

> 我们也可以将查询更改为*created _ at>=*'*2021–02–01 0:00 '和 created _ at<= ' 2021–02–28 11:59:59 '*

让我们通过将 max parallel workers 设置为 0 来运行和分析该查询。

```
**SET** *max_parallel_workers* = 0;**EXPLAIN ANALYZE SELECT** **date_trunc***('day', created_at)*, **avg**(**count**) **FROM** *ticketing_system ts* **WHERE** *created_at* **BETWEEN** *'2021-02-01 0:00'* **AND** *'2021-02-28 11:59:59'* **GROUP** **BY** *1* **ORDER** **BY** *1*;
```

结果是

在我的 PostgreSQL 环境中，执行该查询需要大约***2600 毫秒*** 。请注意，尽管它计划启动两个 并行工作器 ***，但实际上没有一个被启动。让我们看看重新启用并行查询时会发生什么。***

```
**SET** *max_parallel_workers* = 8;**EXPLAIN ANALYZE SELECT** **date_trunc***('day', created_at)*, **avg**(**count**) **FROM** *ticketing_system ts* **WHERE** *created_at* **BETWEEN** *'2021-02-01 0:00'* **AND** *'2021-02-28 11:59:59'* **GROUP** **BY** *1* **ORDER** **BY** *1*;
```

在这种情况下，PostgreSQL 决定启动两个并行工作器，整体查询性能提高了近 1.5 倍。

现在，让我们在 created_at 列上创建索引 BTREE index。

```
**CREATE** **INDEX** *in_ticketing_system_btree* **ON** *ticketing_system(created_at)*;
**SET** *max_parallel_workers* = 0;**EXPLAIN ANALYZE SELECT** **date_trunc***('day', created_at)*, **avg**(**count**) **FROM** *ticketing_system ts* **WHERE** *created_at* **BETWEEN** *'2021-02-01 0:00'* **AND** *'2021-02-28 11:59:59'* **GROUP** **BY** *1* **ORDER** **BY** *1*;
```

BTree 索引的性能优于并行查询。现在让我们看看指数的大小。大小是 853 兆，这是巨大的。

```
**SELECT** pg_size_pretty(pg_relation_size('in_ticketing_system_btree'));----------------------------
pg_size_pretty|
--------------+
853 MB        |
```

现在，是时候在 created_at 上创建 BRIN 索引并测试查询性能了。

```
**DROP** **INDEX** *in_ticketing_system_btree;*
**CREATE** **INDEX** *in_ticketing_system_brin* **ON** *ticketing_system* **USING** *brin(created_at);***EXPLAIN ANALYZE SELECT** **date_trunc***('day', created_at)*, **avg**(**count**) **FROM** *ticketing_system ts* **WHERE** *created_at* **BETWEEN** *'2021-02-01 0:00'* **AND** *'2021-02-28 11:59:59'* **GROUP** **BY** *1* **ORDER** **BY** *1*;
```

BTREE 和 BRIN 索引的查询性能相似，BRIN 索引优于 BTREE 索引。除此之外，BRIN 节省了大量索引空间。

```
**SELECT** *pg_size_pretty(pg_relation_size('in_ticketing_system_brin'));*pg_size_pretty|
--------------+
96 kB         |
```

没错，布林索引只占用**96kb！**这意味着 BRIN 索引**占用很少的空间**来存储相同数据上的 B 树索引，并且在这些分析类型的查询中表现更好。

现在让我们更改查询，以获得一天中每小时的平均人数。

***带 BTREE 索引:***

```
**EXPLAIN** **ANALYZE** **SELECT** **date_trunc***('hour', created_at)*, **avg**(**count**)**FROM** *ticketing_system ts*
**WHERE** *created_at* **BETWEEN** *'2021-02-01 0:00'* **AND** *'2021-02-01 11:59:59'*
**GROUP** **BY** *1* **ORDER** **BY** *1*;
```

***同布林指数:***

这两个索引在性能上几乎相同，但是与 BTREE 相比，BRIN 索引提供了极大的节省空间的好处。

我在 100M 上运行了一个性能测试，布林在处理如此庞大的数据时表现良好。

首先，正如您可能已经观察到的，随着您的表增长到一个相当大的规模，您真正开始看到 BRIN 索引的好处。它还展示了 PostgreSQL 的垂直伸缩能力:BRIN 索引当然可以帮助您高效地运行查询，解决您试图解决的许多问题，尤其是时态分析。

最大的问题是存储空间:如果您的数据集允许您利用 BRIN 索引，那么能够将索引占用空间减少 99%以上就是巨大的。

BRIN 用法将返回特定范围内所有页面中的所有元组。所以索引是有损耗的，需要额外的工作来进一步过滤掉记录。因此，虽然有人可能会说这不好，但还是有一些好处的。

1.  由于只存储一系列页面的摘要信息，所以与 B 树索引相比，BRIN 索引通常非常小。所以如果我们想把工作集的数据挤到 shared_buffer，这是一个很大的帮助。
2.  布林损失可以通过指定每个范围的页数来控制(在后面的部分中讨论)
3.  将汇总工作卸载到真空或自动真空。所以事务/ DML 操作的索引维护开销是最小的。

## 限制:

如果键值的排序遵循存储层中块的组织，那么 BRIN 索引是有效的。在最简单的情况下，这可能需要表的物理顺序(通常是表中行的创建顺序)与键的顺序相匹配。生成的序列号或创建的数据上的键是 BRIN 索引的最佳候选者。

在上面的例子中，由于 created_at 的行是以递增的方式排列的，所以 BRIN 表现得更好。如果有日期被修改，那么 BRIN 不会按预期执行。