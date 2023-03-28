# 如何在 Oracle 云中创建始终免费的 K8S 集群

> 原文：<https://medium.com/geekculture/how-to-create-an-always-free-k8s-cluster-in-oracle-cloud-60be3b107c44?source=collection_archive---------1----------------------->

# 语境

甲骨文云基础设施(OCI)有一个永远免费的计划([在这里](https://www.oracle.com/cloud/free/))，在这里你可以免费**和永远**使用一些计算资源。

其中，您可以访问:

*   两个 AMD 计算虚拟机(1/8 内核和 1gb 内存)
*   多达 4 个 Arm Ampere A1 计算实例( **4 个内核和总共 24gb RAM**)

考虑到这一点，我开始思考如何使用这些工具为小型应用程序创建一个有用的环境…