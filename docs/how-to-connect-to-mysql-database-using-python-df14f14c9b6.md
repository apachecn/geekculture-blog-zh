# 如何使用 python 连接 mysql 数据库

> 原文：<https://medium.com/geekculture/how-to-connect-to-mysql-database-using-python-df14f14c9b6?source=collection_archive---------7----------------------->

## 使用 python 连接 mysql 的不同方式

![](img/696cd2f0aa4247cf0b5ee96d2a6f9bdf.png)

Photo by [Campaign Creators](https://unsplash.com/@campaign_creators?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 简介:

本文讨论如何使用`python`连接到 mysql 数据库。我们将回顾使用 python 连接到`mysql`的三种方式

如果你正在寻找基本`SQL`查询的介绍，请阅读[这篇](https://dock2learn.com/tech/basic-sql-queries-postgresql/)帖子。

# 使用 sql 连接器连接到 mysql:

使用`mysql-connector-python`库可以建立到`mysql`的连接。

```
$pip install mysql-connector-python

Collecting mysql-connector-python
  Using cached mysql_connector_python-8.0.30-cp39-cp39-macosx_11_0_x86_64.whl (5.1 MB)
Collecting protobuf<=3.20.1,>=3.11.0
  Using cached protobuf-3.20.1-cp39-cp39-macosx_10_9_x86_64.whl (962 kB)
Installing collected packages: protobuf, mysql-connector-python
Successfully installed mysql-connector-python-8.0.30 protobuf-3.20.1import mysql.connector

def connect_to_mysql() -> mysql.connector:
    """
    Establish the database connection.
    """

    cnx = mysql.connector.connect(
        user="username",
        password="password",
        host="hostname",
        database="databasename",
        connection_timeout=10000
    )

    return cnx
```

# 使用 mysqlConnectionPool 连接到 MySQL:

```
from mysql.connector.pooling import PooledMySQLConnection, MySQLConnectionPool

def connect_to_mysql_pool() -> MySQLConnectionPool:
    """
    Establish the database connection.
    """

    connection_pool = MySQLConnectionPool(pool_name="mainpool",
                                          pool_size=3,
                                          pool_reset_session=True,
                                          user="username",
                                          password="password",
                                          host="hostname",
                                          database="databasename",
                                          allow_local_infile=True
                                         )
    return connection_pool
```

*   `mysql.connector.pooling`模块实现池化。
*   当向请求者提供连接时，池打开许多连接并处理线程安全。
*   连接池的大小在池创建时是可配置的。此后不能调整其大小。
*   连接池可以在池创建时命名。如果没有给出名称，则使用连接参数生成一个名称。
*   连接池名称可以从连接池或从中获得的连接中检索。
*   要释放从连接池中获得的池化连接，调用它的`close()`方法，就像对任何未池化的连接一样。然而，对于一个池化的连接，`close()`并不真正关闭连接，而是将它返回到池中，并使它可用于后续的连接请求。

# 使用 SQLAlchemy 连接到 mysql:

在大多数项目中，我们将主要使用 ORM 来连接数据库。ORM 不仅方便，而且与数据库无关，这意味着代码更改更少。唯一的变化可能是连接字符串或驱动程序。

我们将使用`pymysql`驱动程序通过 SQL Alchemy 连接到 mysql。

## 安装 pymysql

```
$ pip install pymysql
```

## 使用 SQL Alchemy 连接到 mysql 的代码

```
from sqlalchemy import create_engine
from sqlalchemy.exc import SQLAlchemyError
from sqlalchemy.orm import sessionmaker
from urllib.parse import quote

def connect_to_mysql_orm():

    connection_url = "mysql+pymysql://username:{}hostname/dbname".format(quote("pa$$w0RD@"))

    try:
        engine = create_engine(url=connection_url)

        # create a session
        session = sessionmaker(bind=engine)
        conn = session()

        if session:
            return conn

    except SQLAlchemyError as se:
        print(se)
```

*原载于 2022 年 9 月 28 日 https://dock2learn.com*[](https://dock2learn.com/tech/how-to-connect-to-mysql-database-using-python/)**。**