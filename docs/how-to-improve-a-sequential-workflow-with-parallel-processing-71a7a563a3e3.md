# 如何用并行处理改进顺序工作流

> 原文：<https://medium.com/geekculture/how-to-improve-a-sequential-workflow-with-parallel-processing-71a7a563a3e3?source=collection_archive---------6----------------------->

## 讨论如何改进使用多线程调用外部服务的顺序工作流，以及一种使用线程安全集合处理大规模问题的可能方法。

# 背景

我们经常不得不多次调用同一个外部服务，或者多个外部服务，以便获得处理数据所需的所有数据。最简单的方法…