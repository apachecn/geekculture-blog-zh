# 根据作业队列大小扩展 EC2 实例

> 原文：<https://medium.com/geekculture/scaling-ec2-instances-subject-to-job-queue-size-edb0dbc1e550?source=collection_archive---------13----------------------->

## 如何根据后台作业队列的长度来扩展 EC2 实例

![](img/3c951042de4a6306c17feb98899d4fd9.png)

EC2 自动扩展组被开发人员广泛用于使用各种参数(如 CPU、内存和许多其他 CloudWatch 指标)来扩展他们的应用程序。很多时候，需要根据作业队列的长度来启动/删除实例。