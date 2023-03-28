# 使用 Kubernetes 作业进行批量处理

> 原文：<https://medium.com/geekculture/batch-processing-in-scale-using-kubernetes-jobs-2759fdf9bdb3?source=collection_archive---------4----------------------->

本文讨论了一种方法，可以帮助客户使用 Kubernetes 作业和 AWS step 函数在 AWS EKS 扩展大型文件处理工作负载。

# 介绍

这种方法使用 AWS 步骤功能来编排端到端流程，包括:

*   从 S3 自动气象站读取输入文件
*   将大的输入文件分割成较小的文件，处理它，将数据保存到数据库中
*   将输出文件写入 AWS S3