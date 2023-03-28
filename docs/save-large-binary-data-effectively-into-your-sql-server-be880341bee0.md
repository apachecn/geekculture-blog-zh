# 将大量二进制数据有效地保存到 SQL Server 中

> 原文：<https://medium.com/geekculture/save-large-binary-data-effectively-into-your-sql-server-be880341bee0?source=collection_archive---------1----------------------->

![](img/d10d25673f3823c27d4d40871f8d0989.png)

在 SQL Server 中，许多开发人员存储二进制文件数据，如音频、视频或简单图像。它们有一个共同点，它们将使用 blob 数据(varbinary)来存储这些类型的数据。这样做的问题是，您在这方面受限于 **2GB** 。它还会降低数据库性能，或者整个数据库系统的性能。

# 拯救文件流