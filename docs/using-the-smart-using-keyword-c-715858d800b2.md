# 使用关键字 C#使用“智能”

> 原文：<https://medium.com/geekculture/using-the-smart-using-keyword-c-715858d800b2?source=collection_archive---------12----------------------->

在本文中，我将解释如何在使用关键字的*的帮助下，我们可以安全地在。Net 应用程序。*

我们知道，我们应该从。NET 来清理我们的对象正在管理的任何非托管(或托管，见[这个](https://stackoverflow.com/questions/538060/proper-use-of-the-idisposable-interface))资源，如数据库连接、套接字或窗口句柄。

让我们创建一个控制台应用程序，看看使用和 IDisposable 接口会发生什么情况。