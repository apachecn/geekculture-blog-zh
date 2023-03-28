# 在 Microsoft SQL Server 中管理工作负载

> 原文：<https://medium.com/geekculture/managing-workloads-in-microsoft-sql-server-13ef89ce7880?source=collection_archive---------3----------------------->

首先讲一些基础知识——工作负载是什么意思？

工作负载是以客户端请求的形式发送给应用程序的工作量。就像任何应用程序一样，Microsoft SQL Server 也可以利用工作负载的力量来更好地管理客户端请求。SQL Server (2016 年以后)和 Azure SQL 数据库有一个名为 **SQL Server 资源调控器**的功能，有助于管理服务器工作负载和资源消耗。SQL Server 资源管理器将一组查询/客户机请求分配为一个**工作负载**。然后，它将多个…