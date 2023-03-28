# 事实引擎查询语言(FEQL)中的标识和代理键

> 原文：<https://medium.com/geekculture/identity-and-surrogate-keys-in-factengine-query-language-feql-6371da89348d?source=collection_archive---------48----------------------->

## FactEngine 语义解析工作原理的细微之处

在上面的查询中，一个提供者是通过他们的名字 Kathy Snelling 来标识的，但是 FactEngine 是如何知道一个提供者是通过他们的名字后跟姓氏来标识的呢？为什么不是姓后面跟着名？

如果我们查看查询所操作的模式的实体关系图，我们可以看到提供者实体有一个 Privider_Id 主键和一个…