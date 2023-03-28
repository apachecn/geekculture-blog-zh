# 非批处理、非预定工作负载的气流

> 原文：<https://medium.com/geekculture/airflow-for-non-batch-non-scheduled-workloads-514b7503aa66?source=collection_archive---------33----------------------->

2020 年 12 月[阿帕奇发布了**气流 2.0**](https://airflow.apache.org/blog/airflow-two-point-oh-is-here/) ，引入了很多有趣的新变化。
我觉得更吸引人的是新调度器组件改进带来的[**17 倍加速**](https://www.astronomer.io/blog/airflow-2-scheduler) 。

过去，我一直对 Apache Airflow 的模块化和易用性感兴趣，我想知道是否可以通过为每个用户请求触发 Dag 来将它用于非批处理、非调度的大规模工作负载。([相关 stackoverflow 问题](https://stackoverflow.com/questions/60082546/airflow-proper-way-to-run-dag-for-each-file))。
新的日程安排要求进一步探讨这个话题。