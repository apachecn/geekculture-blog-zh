# 关于 Microsoft SQL Server 中存储的进程、锁、事务提交和查询计划的初级读本

> 原文：<https://medium.com/geekculture/a-primer-on-stored-procs-locks-transaction-commits-query-plans-in-microsoft-sql-server-5be28495c085?source=collection_archive---------12----------------------->

很多时候，我们可能会在一个或多个报告中听到业务涉众关于速度的问题。当我们深入研究时，我们可能会发现存储过程返回结果的速度不如预期的快。在此期间，我们需要优化报告使用的存储过程或后端查询，以便更快地返回结果。

但是在学习如何优化存储过程之前，理解 SQL 提供的存储过程类型是很重要的。