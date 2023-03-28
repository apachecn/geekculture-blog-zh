# Spring @事务回滚处理

> 原文：<https://medium.com/geekculture/spring-transactional-rollback-handling-741fcad043c6?source=collection_archive---------0----------------------->

![](img/5f8e11008c3fc985cda63a3ea3fb8086.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

默认情况下，春季引导事务是**自动提交**。每一条 SQL 语句都在自己的事务中，并将在执行后提交。看一下下面的例子，即使出现了异常，产品还是被插入到数据库中。

```
public void createProduct() {  
    System.out.println("------ createProduct ------");
    Product prod = new Product()…
```