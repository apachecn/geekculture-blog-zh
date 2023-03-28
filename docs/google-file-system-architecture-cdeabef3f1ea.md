# 谷歌文件系统架构

> 原文：<https://medium.com/geekculture/google-file-system-architecture-cdeabef3f1ea?source=collection_archive---------1----------------------->

![](img/cb837b073e4b7f96251ae2aac59475a7.png)

Photo by [Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 学习分布式系统的过程是不可避免的，这是一个经典案例。

目前的分布式文件系统有 [**RedHat 的 GFS**](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/global_file_system/ch-overview-gfs) (全局文件系统) [**IBM 的 GPFS**](https://www.ibm.com/products/spectrum-scale)[**Lustre 文件系统**](https://www.lustre.org/) 用于高性能计算或大型数据中心，对硬件设施要求较高。谷歌文件系统…