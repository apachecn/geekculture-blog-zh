# 跟踪 SQL Server 数据库中的更改—简要指南

> 原文：<https://medium.com/geekculture/a-brief-guide-to-tracking-mechanisms-in-sql-server-databases-8616c2906d?source=collection_archive---------9----------------------->

## **概述——为什么需要跟踪变更？**

跟踪更改有助于我们了解谁做了哪些更改，从而确保我们保存的数据更加准确。

变更跟踪机制可以由开发人员开发，但是这将涉及足够的开发变更和对模式的修改，从而导致大量的开发工作。它还会有很大的性能开销。因此，SQL Server 提供了不同的内置跟踪机制，如-