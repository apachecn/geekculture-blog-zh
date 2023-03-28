# 如何使用 Spring JDBC 在不创建连接对象的情况下更新 MySQL 表

> 原文：<https://medium.com/geekculture/how-to-use-spring-jdbc-to-update-mysql-tables-without-creating-connection-object-6d004392fba5?source=collection_archive---------6----------------------->

![](img/44a11b0f334c6a4f666bad1cd8f09828.png)

Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您在 Spring project 中使用 JDBC 模板，并且您有 SQL 数据操作语言(DML)语句，如 UPDATE、DELETE 和 INSERT 语句，大多数网站会告诉您使用数据库 URL、用户名和密码创建连接对象，如下所示:

```
Connection con = DriverManager.getConnection(
```