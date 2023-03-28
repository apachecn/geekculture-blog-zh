# Python 名称空间和范围

> 原文：<https://medium.com/geekculture/python-namespaces-and-scopes-acd1b2da9797?source=collection_archive---------8----------------------->

## Python 如何记忆

![](img/d92275d5ba76a2ddc7b08785b7f05749.png)

**The Python Logo from** [**Python.org**](https://www.python.org/community/logos/)

想知道 Python 是如何跟踪所有被赋予符号名称的对象的吗？或者为什么存储在函数中特定符号名下的对象不会影响存储在函数外同名的对象？我们来谈谈 Python 中的**名称空间**。

名称空间是一种抽象，用于组织分配给对象的符号名称…