# SQLAlchemy 入门

> 原文：<https://medium.com/geekculture/getting-started-with-sqlalchemy-d132d04c940?source=collection_archive---------8----------------------->

## 用 Python 实现数据库操作的 SQLAlchemy

![](img/abbe9040695b6e0a7246ff44b06358f8.png)

Image [Reference](/swlh/orm-and-sqlalchemy-the-magic-wand-in-database-management-a2d02877a57a): Medium

## SQLAlchemy 是什么？

SQLAlchemy 是一个用于 Python 的开源 SQL 工具包和对象关系映射器(ORM)。

它促进了 Python 程序和数据库之间的通信。大多数时候，这个库被用作 ORM 工具。

**什么是 ORM？**

ORM 是一种编程技术，用于在编程语言中将数据库记录表示为相应的对象。因此，我们可以在这些数据库记录上进行交互和操作，就像我们在处理对象一样。

ORM 让我们有能力**将数据库记录转化为对象**，使得数据库交互和修改像在编程语言中处理对象一样轻松。

![](img/550e2fdc40207390e68ad456778fdf30.png)

[Image Reference](https://www.fullstackpython.com/)

**ORM 有哪些利弊？**

**优点**

*   **简洁。** ORM 允许用户使用他们选择的编程语言，其语法比使用原始 SQL 查询更简洁，减少了所需的代码量。
*   **最优。通过将数据库记录表示为对象，ORM 还使用户能够利用面向对象编程的优化，如继承、封装和抽象。**
*   **灵活性。**使用 ORM，用户可以轻松地在不同的数据库管理系统(MySQL、Sqlite3、PostgreSQL……)之间切换，而无需对这些系统和 SQL 语言有很好的理解。

**缺点**

*   **时间。因为 ORM 是框架，用户必须花时间学习和熟悉这些工具。**
*   **较少控制。**使用 ORM，用户对数据库的控制权和主动权将会减少。

> **让我们学习如何使用 SQLAlchemy，**这是 Python 编程语言中一个相当著名的 ORM，看看这个工具如何帮助您在处理数据库时节省大量的时间和精力。

![](img/834e34f87cec2c50dcf423c707cd5c76.png)

## 安装

```
!pip install sqlalchemy
!pip install pymysql
```

## 导入:

```
try:
    import sqlalchemy as db
    from sqlalchemy import create_engine
    from sqlalchemy import *
    import pymysql
    print("all imported")

except:
    print("error in importing")
```

## SQLAlchemy 中的引擎配置

根据 SQLAlchemy 文档:引擎是任何 SQLAlchemy 应用程序的起点。它是实际数据库及其 DBAPI 的“大本营”,通过连接池和方言交付给 SQLAlchemy 应用程序，后者描述了如何与特定种类的数据库/DBAPI 组合进行对话。

一般结构可以说明如下:

![](img/f1ffa17bf187b56aa7d89d584fb231ff.png)

[Image Reference](https://docs.sqlalchemy.org/en/14/core/engines.html)

在上面的例子中，一个引擎引用一个方言和一个池，它们一起解释 DBAPI 的模块功能以及数据库的行为。

创建引擎只是发出一个调用 create_engine()的问题。我们正在使用的数据库的 API 方言在 create_engine 中传递。

## 对于 PostgreSQL

## 默认值:

> engine = create _ engine(' PostgreSQL://Scott:tiger @ localhost/my database ')

## 其他:

> engine = create _ engine(' PostgreSQL+psycopg 2://Scott:tiger @ localhost/my database ')
> 
> engine = create _ engine(' PostgreSQL+pg 8000://Scott:tiger @ localhost/my database ')

## 对于 MySQL

## 默认

> engine = create _ engine(' MySQL://Scott:tiger @ localhost/foo ')

## 其他:

> mysqlclient

*   engine = create _ engine(' MySQL+mysqldb://Scott:tiger @ localhost/foo ')

> PyMySQL

*   engine = create _ engine(' MySQL+pymysql://Scott:tiger @ localhost/foo ')

## 其他数据库也是如此:

## 甲骨文:

> engine = create _ engine(' Oracle://Scott:tiger @ 127 . 0 . 0 . 1:1521/sid name ')

## Microsoft SQL Server

> engine = create _ engine(' MSSQL+pymssql://Scott:tiger @ hostname:port/dbname ')

## SQLite

SQLite 连接到基于文件的数据库，默认情况下使用 Python 内置模块 sqlite3。由于 SQLite 连接到本地文件，URL 格式略有不同。URL 的“文件”部分是数据库的文件名。对于相对文件路径，这需要三个斜杠。三个斜线后面是绝对路径:

> engine = create _ engine(' SQLite:///foo . db ')

## 让我们开始并连接到我的 Mysql 数据库:

> `format for mysql : create_engine('mysql+pymysql://<username>:<password>@<host>/<dbname>')
> 
> dialect+driver : mysql+pymysql`

```
engine = db.create_engine('mysql+pymysql://root:shelvi31@127.0.0.1/errors?host=localhost?port=3306')connection = engine.connect()
print(engine.table_names())
```

## 反射:读取数据库并构建 SQLAlchemy 表对象

SQLAlchemy 可以用来通过反射从数据库中自动加载表。反射是读取数据库并基于该信息构建元数据的过程。

导入反射所需的元数据和表。元数据是一个对象，是一个存储数据库信息的目录，比如一个表，这样我们就不需要一直查找它们。

```
from sqlalchemy import MetaData,Table 
metadata = db.MetaData()**#Defining table object by giving name of table stores in db:**
dummy = Table("dummy",metadata,autoload = True, autoload_with=engine)**#Using Function repr to view the details of the table that we stored as dummy:**
print(repr(dummy));**# Print only the column names**
print(dummy.columns.keys())
```

请注意，在调用 Engine.connect()方法或调用依赖于此方法的操作(如 Engine.execute())之前，引擎及其底层池不会建立第一个实际的 DBAPI 连接。

## 询问

```
query = db.select([dummy]).where(dummy.columns.seic >= 20)
result = connection.execute(query)
resultset = result.fetchall()
resultset[0:4]**#Converting the result database to df:**
import pandas as pd 
df = pd.DataFrame(resultset)
df.columns = resultset[0].keys()
df.head(10)
```

![](img/9d2ee6e3e54c56dc44e523bf114250ac.png)

## 执行更多查询

## 使用位置:

```
query = db.select([dummy.columns.seic,dummy.columns.seic_benefit]).where(dummy.columns.seic >= 70)
result = connection.execute(query)

resultset = result.fetchall()
resultset[:10]
```

## 使用于:

```
query = db.select([dummy.columns.seic,dummy.columns.seic_benefit]).where(dummy.columns.seic.in_([72.6,70,60]))

result = connection.execute(query)

resultset = result.fetchall()
resultset[:][(60.0, '89664'), (60.0, '17082'), (70.0, '76108'), (60.0, '27696')]
```

## 和，或者，不是

```
# SQL :
# SELECT * FROM dummy
# WHERE seic = 70 AND NOT scheme1_benefit = 'Eligible'

# SQLAlchemy :
query = db.select([dummy.columns.seic,dummy.columns.scheme1_benefit]).where(db.and_(dummy.columns.seic >= 80, dummy.columns.scheme1_benefit != 'Eligible'))
result = connection.execute(query)

resultset = result.fetchall()
resultset[:]# SQL :
# SELECT * FROM dummy
# WHERE seic = 70 OR NOT scheme1_benefit = 'Eligible'

# SQLAlchemy :
query = db.select([dummy.columns.seic,dummy.columns.scheme1_benefit]).where(db.or_(dummy.columns.seic >= 80, dummy.columns.scheme1_benefit != 'Eligible'))
result = connection.execute(query)
resultset = result.fetchall()
resultset[:]
```

## 排序依据:

```
# SQL :
# SELECT * FROM dummy
# ORDER BY seic DESC, seic_benefit DESC

# SQLAlchemy :
query = db.select([dummy]).where(dummy.columns.seic==60).order_by(db.desc(dummy.columns.seic), dummy.columns.seic_benefit)

result = connection.execute(query)
resultset = result.fetchall()
resultset[:10]
```

## 功能:

```
# SQL :
# SELECT COUNT(seic)
# FROM dummy
# WHERE seic==70

# SQLAlchemy :
query = db.select([db.func.count(dummy.columns.seic)]).where(dummy.columns.seic==60)
result = connection.execute(query)
resultset = result.fetchall()
resultset[:10][(3,)]
```

其他功能包括平均值，总和，最小值，最大值…

## 分组依据:

```
# SQL :
# SELECT SUM(seic) as SEIC
# FROM dummy
# GROPU BY scheme1_benefit

# SQLAlchemy :
query= db.select([db.func.sum(dummy.columns.seic).label('SEIC')]).group_by(dummy.columns.scheme1_benefit)
result = connection.execute(query)
resultset = result.fetchall()
resultset[:5][(13828.999965667725,), (15699.400007247925,)]
```

## 明显的

```
# SQL :
# SELECT DISTINCT seic
# FROM dummy

# SQLAlchemy :
query = db.select([db.func.count(dummy.columns.seic.distinct())])
result = connection.execute(query)
resultset = result.fetchall()
resultset[:5][(197,)]
```

在这里找到我的 Jupyter 笔记本和数据集:[https://github.com/shelvi31/SQLALchemy-in-python](https://github.com/shelvi31/SQLALchemy-in-python)

## 参考资料:

1.  [https://towardsdatascience . com/sqlalchemy-python-tutorial-79a 577141 a91](https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91)
2.  [https://docs.sqlalchemy.org/en/14/core/engines.html](https://docs.sqlalchemy.org/en/14/core/engines.html)
3.  [https://medium . com/swlh/ORM-and-sqlalchemy-the-magic-wand-in-database-management-a2d 02877 a57a](/swlh/orm-and-sqlalchemy-the-magic-wand-in-database-management-a2d02877a57a)