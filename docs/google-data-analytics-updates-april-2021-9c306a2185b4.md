# 谷歌数据分析的最新更新(2021 年 4 月)

> 原文：<https://medium.com/geekculture/google-data-analytics-updates-april-2021-9c306a2185b4?source=collection_archive---------21----------------------->

## BigQuery、Data Studio、Google Analytics (GA)和 Google Tag Manager (GTM)的更新亮点。亚历山大·柯俊

![](img/f1e48a0a23d513d94f931427f4932a5e.png)

Photo by [Yolanda Sun](https://unsplash.com/@iyolanda) on [Unsplash](https://unsplash.com/)

在这篇博文中，我想总结一下我们每天在 [datadice](https://www.datadice.io/) 使用的谷歌工具的新版本。因此，我想对 BigQuery、Data Studio、Google Analytics 和 Google Tag Manager 的新特性做一个概述。此外，我将重点介绍我认为最重要的几个版本，还会列举一些其他的改动。

如果你想仔细看看，这里可以找到来自 [BigQuery](https://cloud.google.com/bigquery/docs/release-notes) 、 [Data Studio](https://support.google.com/datastudio/answer/10331528?hl=en) 、[Google Analytics](https://support.google.com/analytics/answer/9164320?hl=en)&[Google Tag Manager](https://support.google.com/tagmanager/answer/4620708?hl=en)的发布说明

# BigQuery

在 BigQuery 中，现在可以很容易地将非聚集表转换为聚集表，反之亦然。此外，还可以更改表聚集所依据的列的数量和类型。在此更改之前，您必须删除原始表，然后创建一个具有相同名称和新聚集列的新表。

使用标准 SQL，您现在可以使用 ALTER TABLE DROP COLUMN 删除表中的单个列(记录中的分区列、聚集列和嵌套列除外)。

此外，列名和 UDF 名称的最大长度现在从 128 个字符增加到 300 个字符。

# 数据工作室

在 Data Studio 中，现在您可以使用这 3 个函数

*   联合
*   IFNULL
*   努里夫

**合并**

您键入数据源的可用字段列表，并返回第一个非空字段值。

**如果为空**

该函数接收一个字段作为第一个参数，如果该字段值为 NULL，则返回第二个参数的值，否则返回第一个参数的值。

**NULLIF**

NULLIF 的工作方式正好相反，所以当第一个参数的值与表达式匹配时，它返回 NULL，否则返回输入参数。

这些函数非常好，在特殊情况下，数据源字段的处理更容易。但是同样的功能也可以用 CASE 构建——WHEN 或 IF 函数。

Data Studio 现在在视图模式的右上角有一个新按钮。使用该按钮，您可以重置您在报告上设置的所有过滤器。因此，当您在重置后第一次访问整个报告时，它具有相同的状态。

![](img/0d55fb43609b57803e330c6df77a705a.png)

*the reset button in the menu*

另一个很好的改进是现在在嵌入式报告中工作的社区可视化。

# 谷歌分析

谷歌分析没有大的更新，只是一些新的指标，特别是对游戏应用的分析。此外，Google 现在提供了新的方法来衡量 GA4 数据 BigQuery 导出中的用户参与度。

# 谷歌标签管理器

没有新的功能发布，他们只是写了一个安装指南，以建立服务器端跟踪。你可以在这里看一看[。](https://developers.google.com/tag-manager/serverside/manual-setup-guide)

# 更多链接

这篇文章是来自 [datadice](https://www.datadice.io/) 的谷歌数据分析系列的一部分，每月向你解释 BigQuery、Data Studio、谷歌分析和谷歌标签管理器的最新功能。

如果你想了解更多关于如何使用 Google Data Studio 并结合 BigQuery 更上一层楼，请查看我们的 Udemy 课程[这里](https://www.udemy.com/course/bigquery-data-studio-grundlagen/?referralCode=49926397EAA98EEE3F48)