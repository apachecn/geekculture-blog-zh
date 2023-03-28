# 部署您自己的麋鹿群

> 原文：<https://medium.com/geekculture/deploying-your-own-elk-cluster-8ec979360a4?source=collection_archive---------1----------------------->

## 弹性认证工程师之路

![](img/e0fe39269944bdf8b355bedb1821fce0.png)

Photo by [İsmail Enes Ayhan](https://unsplash.com/@ismailenesayhan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/computer-server?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我真的很喜欢将 ELK 栈用于多种事情和项目，[主要集中在安全性上](https://infosecwriteups.com/secure-network-monitoring-with-elastic-packetbeat-suricata-7ecd871b1a52)。但是为了使用 ELK，一个主要的必要条件是部署一个弹性搜索集群。在本指南中，我将解释**如何部署、配置和保护您的集群**。还有，有必要说一下，这个指南是为 **Elasticsearch 7.2** 写的。