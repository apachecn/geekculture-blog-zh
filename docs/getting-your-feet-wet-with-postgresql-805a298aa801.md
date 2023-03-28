# 试用 postgresql

> 原文：<https://medium.com/geekculture/getting-your-feet-wet-with-postgresql-805a298aa801?source=collection_archive---------18----------------------->

## 理解 SQL 的基本查询

![](img/97cf3fd15e1a568d501bee0c52568e6e.png)

Creator: mirsad sarajlic | Credit: Getty Images/iStockphoto

# 简介:

本文讨论并向您介绍了我们在日常工作中使用的基本而重要的`SQL`查询。尽管今天的大多数项目都使用类似于`[SQLAlchemy](https://www.sqlalchemy.org/)`的[ORM](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping)，但是知道如何编写基本的原始`SQL`查询还是很有好处的。

当 ORM 的固有方法或过滤器有限，并且您需要构造一个复杂的查询时，理解原始的`SQL`查询也可以派上用场。

尽管大多数 SQL 查询在不同的数据库中在语法上是相似的，但是很少有内置函数的名称是不同的。在这篇文章中，我们将`postgresql`作为数据库，我们将对它执行查询。

# 要求:

*   公司自产自用
*   mac osx

# 设置 postgresql:

我们将使用`homebrew`在 mac 上安装 postgresql。

## 安装 postgresql

```
$brew install postgresql 
==> Downloading [https://ghcr.io/v2/homebrew/core/krb5/manifests/1.20](https://ghcr.io/v2/homebrew/core/krb5/manifests/1.20) ######################################################################## 100.0% ==> Installing dependencies for postgresql: krb5 ==> Installing postgresql dependency: krb5 ==> Pouring krb5--1.20.monterey.bottle.tar.gz 🍺 /usr/local/Cellar/krb5/1.20: 162 files, 5.1MB ==> Installing postgresql ==> Pouring postgresql--14.4.monterey.bottle.tar.gz ==> /usr/local/Cellar/postgresql/14.4/bin/initdb --locale=C -E UTF-8 /usr/local/var/postgres ==> Caveats ==> postgresql To migrate existing data from a previous major version of PostgreSQL run: brew postgresql-upgrade-database This formula has created a default database cluster with: initdb --locale=C -E UTF-8 /usr/local/var/postgres For more details, read: [https://www.postgresql.org/docs/14/app-initdb.html](https://www.postgresql.org/docs/14/app-initdb.html) To restart postgresql after an upgrade: brew services restart postgresql
```

## 启动 postgresql

```
$brew services start postgresql 
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```

## 停止 postgresql

```
$brew services start postgresql
```

# 创建数据库:

下面的查询创建了一个数据库并连接到它。`\c`是连接数据库的命令。只要给`\c`就会以默认用户的身份连接到默认数据库。`\dt`将显示该数据库中的所有表，就像`mysql`中的`show tables`一样。

```
$psql postgres psql 
(14.4) Type "help" for help. postgres=#create database expenses; CREATE DATABASE postgres-# \c expenses dinesh; You are now connected to database "expenses" as user "dinesh".
```

现在让我们假设已经创建了一个表，并且其中有数据。表格如下所示。

```
user_id |  name  |  product  | price 
---------+--------+-----------+-------
       1 | Dinesh | Microwave |  9000
       2 | user2  | Table     |  9000
       3 | user1  | Table     |  3000
       4 | user4  | TV        | 60000
       6 | John   | Chair     |  3000
       7 | Tim    | Chair     |   300
       8 | Tom    | Wardrobe  |  3000
       9 | Jim    | Cot       |  2000
      10 | user3  | toys      |   900
```

# 选择查询:

`SELECT`查询用于从数据库表中检索数据。这是相当普遍的，大多数开发人员肯定会至少执行一次。

```
expenses=> select * from myexpense; user_id |  name  |  product  | price 
---------+--------+-----------+-------
       1 | Dinesh | Microwave |  9000
       2 | user2  | Table     |  9000
       3 | user1  | Table     |  3000
       4 | user4  | TV        | 60000
       6 | John   | Chair     |  3000
       7 | Tim    | Chair     |   300
       8 | Tom    | Wardrobe  |  3000
       9 | Jim    | Cot       |  2000
      10 | user3  | toys      |   900
(9 rows)
```

字符`*`从指定的表中检索每一列。由于我们还没有指定一个`WHERE`子句，这也将返回每一行。当您在终端上工作时，使用`*`是可以接受的。但是，如果您将此查询用作代码的一部分，建议使用单独的列名以提高可读性。

```
expenses=> select user_id,name,product,price from myexpense; user_id |  name  |  product  | price 
---------+--------+-----------+-------
       1 | Dinesh | Microwave |  9000
       2 | user2  | Table     |  9000
       3 | user1  | Table     |  3000
       4 | user4  | TV        | 60000
       6 | John   | Chair     |  3000
       7 | Tim    | Chair     |   300
       8 | Tom    | Wardrobe  |  3000
       9 | Jim    | Cot       |  2000
      10 | user3  | toys      |   900
(9 rows)
```

我们还可以通过在 select 命令中指定列名来只检索列的子集。

# WHERE 子句:

如果您必须检索行的子集，您可以使用一个`WHERE`子句来这样做。

```
expenses=> select user_id,name,product,price from myexpense where product='Chair'; user_id | name | product | price 
---------+------+---------+-------
       6 | John | Chair   |  3000
       7 | Tim  | Chair   |   300
(2 rows)
```

# 将多个条件应用于查询:

假设我们需要检索满足多个条件的查询，我们可以将`AND`和`OR`与`WHERE`子句一起使用。

例如，如果我们需要检索 chair 的所有记录以及价格低于 3000 的记录，我们应该使用`OR`操作符。

```
expenses=> select user_id,name,product,price from myexpense where product='Chair' or price <= 3000; user_id | name  | product  | price 
---------+-------+----------+-------
       3 | user1 | Table    |  3000
       6 | John  | Chair    |  3000
       7 | Tim   | Chair    |   300
       8 | Tom   | Wardrobe |  3000
       9 | Jim   | Cot      |  2000
      10 | user3 | toys     |   900
(6 rows)
```

类似地，如果我们需要检索价格高于 300 的椅子的所有记录，我们可以使用`AND`条件。

```
expenses=> select user_id,name,product,price from myexpense where product='Chair' and price > 300; user_id | name | product | price 
---------+------+---------+-------
       6 | John | Chair   |  3000
```

注意:请注意，使用`OR`时，将检索满足任一条件的记录，而使用`AND`时，两个指定的条件都应满足。

# 重命名列:

如果您希望为检索到的列提供不同的名称，我们可以使用`AS`关键字。

```
expenses=> select user_id,name as user,product as item,price as amount from myexpense; user_id |  user  |   item    | amount 
---------+--------+-----------+--------
       1 | Dinesh | Microwave |   9000
       2 | user2  | Table     |   9000
       3 | user1  | Table     |   3000
       4 | user4  | TV        |  60000
       6 | John   | Chair     |   3000
       7 | Tim    | Chair     |    300
       8 | Tom    | Wardrobe  |   3000
       9 | Jim    | Cot       |   2000
      10 | user3  | toys      |    900
(9 rows)
```

# 串联列:

既然我们已经学习了所有的基本查询，让我们来尝试一些复杂的查询。

假设我们需要来自多个列的数据作为单个列，我们可以使用`||`管道操作符。我们需要的输出是名为`result`的单列`user has purchased product` 。`user1 has purchased table`以此类推。

```
expenses=> select name|| ' has purchased ' ||product as msg from myexpense; msg               
--------------------------------
 Dinesh has purchased Microwave
 user2 has purchased Table
 user1 has purchased Table
 user4 has purchased TV
 John has purchased Chair
 Tim has purchased Chair
 Tom has purchased Wardrobe
 Jim has purchased Cot
 user3 has purchased toys
(9 rows)
```

> ***注意事项 1:确保*** `***AS***` ***子句在*** `***FROM***` ***语句之前。否则，将首先执行 FROM 语句，并在查询执行*** `***AS***` ***语句时给出一个临时列名。***

让我们看看如果在 from 语句后放置 AS 会发生什么。

```
expenses=> select name|| ' has purchased ' ||product from myexpense as msg; ?column?            
--------------------------------
 Dinesh has purchased Microwave
 user2 has purchased Table
 user1 has purchased Table
 user4 has purchased TV
 John has purchased Chair
 Tim has purchased Chair
 Tom has purchased Wardrobe
 Jim has purchased Cot
 user3 has purchased toys
(9 rows)
```

你看到了吗？`AS`在这里没有作用。我们将在内联查询中详细讨论这一点。

# 在查询中使用条件逻辑:

现在，我们需要在名为 budget 的列中显示价格为> 3000 的`Overpriced`和价格为< 3000 的`Economical`。我们将使用`case when..then`语句来实现这一点。

```
expenses=> select user_id,name,product,price,case when price > 3000 then 'Overpriced' when price <= 3000 then 'Economical' end as budget from myexpense; user_id |  name  |  product  | price |   budget   
---------+--------+-----------+-------+------------
       1 | Dinesh | Microwave |  9000 | Overpriced
       2 | user2  | Table     |  9000 | Overpriced
       3 | user1  | Table     |  3000 | Economical
       4 | user4  | TV        | 60000 | Overpriced
       6 | John   | Chair     |  3000 | Economical
       7 | Tim    | Chair     |   300 | Economical
       8 | Tom    | Wardrobe  |  3000 | Economical
       9 | Jim    | Cot       |  2000 | Economical
      10 | user3  | toys      |   900 | Economical
(9 rows)
```

# 限制和随机记录:

`limit`和`random()`配合`order by`功能分别用于限制和检索随机记录。

## 限制:

```
expenses=> select user_id,name,product,price from myexpense limit 5; user_id |  name  |  product  | price 
---------+--------+-----------+-------
       1 | Dinesh | Microwave |  9000
       2 | user2  | Table     |  9000
       3 | user1  | Table     |  3000
       4 | user4  | TV        | 60000
       6 | John   | Chair     |  3000
(5 rows)
```

## 随机:

```
expenses=> select user_id,name,product,price from myexpense order by random() limit 5; user_id | name  | product | price 
---------+-------+---------+-------
       7 | Tim   | Chair   |   300
       4 | user4 | TV      | 60000
      10 | user3 | toys    |   900
       3 | user1 | Table   |  3000
       9 | Jim   | Cot     |  2000
(5 rows)
```

# 搜索模式:

为了检索匹配特定模式或子串的行，我们将结合使用`LIKE`操作符和通配符操作符`%`。

我们将检索所有以 c 开头的产品名称。

```
expenses=> select user_id,name,product,price from myexpense where product like 'C%'; user_id | name | product | price 
---------+------+---------+-------
       6 | John | Chair   |  3000
       7 | Tim  | Chair   |   300
       9 | Jim  | Cot     |  2000
(3 rows)
```

我们还可以将条件操作符与模式混合和匹配。现在，让我们用一个`OR`条件重复同样的查询。

```
expenses=> select user_id,name,product,price from myexpense where product like 'C%' or name like 'D%'; user_id |  name  |  product  | price 
---------+--------+-----------+-------
       1 | Dinesh | Microwave |  9000
       6 | John   | Chair     |  3000
       7 | Tim    | Chair     |   300
       9 | Jim    | Cot       |  2000
(4 rows)
```

# 内嵌查询:

我们已经讨论过，我们可以用自己选择的名称来重命名列。如果我们想使用别名列名来过滤行，该怎么办？

```
expenses=> select user_id,name as user,product as item,price as amount from myexpense where amount > 3000; ERROR:  column "amount" does not exist
LINE 1: ...duct as item,price as amount from myexpense where amount > 3...
```

是的，这就是我所说的。那么我们如何处理这些情况呢？

在这种情况下，我们可以形成内联查询。

```
expenses=> select * from (select user_id,name as user,product as item,price as amount from myexpense)x where amount > 3000; user_id |  user  |   item    | amount 
---------+--------+-----------+--------
       1 | Dinesh | Microwave |   9000
       2 | user2  | Table     |   9000
       4 | user4  | TV        |  60000
(3 rows)
```

别名查询将成为主查询的子查询。我们的子查询在这里被命名为 x。where 子句应用于内联查询的结果。

***告诫二:我们为什么要这么做？如前所述，*** `***where***` ***子句是在*** `***select***` ***之前被求值的。所以当执行*** `***where***` ***子句时，别名如 user、item、amount 列不可用。但是，*** `***from***` ***子句是在*** `***where***` ***子句之前求值的。因此，首先执行内联查询，然后我们能够在 where*** `***clause***` *中使用别名列名。*

# 总结:

这里讨论的所有 SQL 查询都是最基本的，可以扩展以编写更复杂的查询。这些基本原理对于从数据库中分割数据非常重要。

尽管所有这些查询都是为 postgresql 编写的，但它们中的大多数也可以用于其他数据库，如`MySQL`、`DB2`、`Oracle`等。

*原载于 2022 年 7 月 14 日*[*https://dock2learn.com*](https://dock2learn.com/tech/basic-sql-queries-postgresql/)*。*