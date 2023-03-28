# 使用 Apache Parquet 分割数据集

> 原文：<https://medium.com/geekculture/partition-dataset-using-apache-parquet-64183bf571ac?source=collection_archive---------2----------------------->

## 使用数据集—第 4 部分:使用 Apache Parquet 对数据集进行分区

数据科学中更常见的任务之一是监控决策策略，该策略在生产中结合了一个或多个机器学习模型。有许多技术来监控决策策略，但是没有数据是不可能的，尤其是在周期性基础上增量加载的数据(例如，每日批处理基础)。

这方面的一个例子是通过决策对客户账户进行的日常处理…