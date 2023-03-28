# AWS 成本节约练习！

> 原文：<https://medium.com/geekculture/aws-cost-saving-exercise-10d0ab0370ff?source=collection_archive---------8----------------------->

## 调度 EC2 实例的启动和停止

![](img/e160bada78f588fa7118e63f3ca87338.png)

Photo by [stevepb](https://pixabay.com/users/stevepb-282134/) on [Pixabay](https://pixabay.com/)

我们公司大量使用 Amazon AWS 进行开发和研发。我们主要在工作时间使用 EC2 实例。如你所知，AWS 是按使用量收费的，我不明白为什么我们要在晚上和周末不用的时候付费。我想出了一个很好的自动启动和关闭 EC2 实例的方法，基于…