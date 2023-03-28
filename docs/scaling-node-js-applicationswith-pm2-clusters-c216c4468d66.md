# 使用 PM2 集群扩展 Node.js 应用程序

> 原文：<https://medium.com/geekculture/scaling-node-js-applicationswith-pm2-clusters-c216c4468d66?source=collection_archive---------1----------------------->

![](img/7136bb0c42fda2231a0ffbb1a99eaed7.png)

Photo by [Taylor Vick](https://unsplash.com/@tvick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/servers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，让我们看看如何使用 PM2 和集群轻松扩展我们的节点应用程序来处理增加的流量负载，而无需修改我们现有的应用程序代码。

# PM2:简明入门:

[PM2](https://pm2.io/) 是一个运行时进程管理和监控工具，带有一个*内置的负载均衡器*，用于 Node.js 应用程序，使得…