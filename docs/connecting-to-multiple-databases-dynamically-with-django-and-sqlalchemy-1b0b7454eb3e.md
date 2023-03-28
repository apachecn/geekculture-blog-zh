# 用 Django 和 Sqlalchemy 动态连接多个数据库

> 原文：<https://medium.com/geekculture/connecting-to-multiple-databases-dynamically-with-django-and-sqlalchemy-1b0b7454eb3e?source=collection_archive---------0----------------------->

![](img/5ff5f78e50ecfd5c190501387fe99049.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我们遇到某些问题时，我们可能需要一些非常规的技巧。在这里，我将讨论使用 sqlalchemy 作为 django ORM 的替代品，在同一个数据库服务器的多个数据库中连接和创建表。

# 问题是

问题很简单，数据库架构是有一个 mysql 数据库服务器和多个数据库。

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| dummy              |
| mysql              |
| mytest             |
| mytest1            |
| mytest2            |
| mytest3            |
| performance_schema |
| sys                |
| test               |
+--------------------+
10 rows in set (0.00 sec)
```

这里，我的测试，我的测试 1，我的测试 2，我的测试 3 是数据库。

在 django 中，你不能动态地连接多个数据库，因为你的连接在 settings.py 文件中。

```
# https://docs.djangoproject.com/en/2.0/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'OPTIONS': {
            'read_default_file': '/etc/mysql/my.cnf',
        },
    }
}
```

my.cnf 文件

```
[client]
database = db_name
user = db_user
password = db_password
default-character-set = utf8
```

在这里，我们只能连接到固定数量的数据库，它们是静态的。

所以我们将通过使用一个流行的库 sqlalchemy 来解决这个问题。

首先安装 sqlalchemy。

```
pip install sqlalchemy
```

我们使用 sqlalchemy 来连接数据库。创建 connect inside views.py 或创建新文件并将其导入视图。

```
def connect(database_name): connection_string = "mysql://{}:    {}@{}/{}".format(db_user,db_password,db_url,database_name) engine = create_engine(connection_string,pool_recycle=3600) connection = engine.connect() return connection
```

接下来，我创建了 alchemymodel.py 文件，它充当 model.py

```
from sqlalchemy.ext.declarative import declarative_basefrom sqlalchemy import Column, Integer, String ,TextBase = declarative_base()class Post(Base):
 __tablename__ = 'post' id = Column(Integer, primary_key=True) name = Column(String(30)) description = Column(Text())
```

在 views.py 中导入该文件

```
from dash.alchemymodel import Base,Postfrom sqlalchemy.orm import sessionmakerfrom rest_framework import status @api_view(['POST'])def PostView():
 data = request.data engine = connect(req_data['database_name']) Base.metadata.create_all(engine) Session = sessionmaker(bind=engine) session = Session() if request.method == 'POST': post = Post(name = "My post" , description = "This is a   sample post") session.add(post) session.commit() Response({'status':'created'}, status = status.HTTP_200_OK)
```

`Base.metadata.creat_all()`在每个数据库中创建表格。

您可以使用 sqlalchemy ORM 插入和查询数据。有关更多信息，请参见 sqlalchemy 的官方文档。

[https://docs.sqlalchemy.org/en/13/orm/](https://docs.sqlalchemy.org/en/13/orm/)

我还使用了 marshmallow 来序列化和反序列化数据库对象，但是这里没有对它们进行解释。