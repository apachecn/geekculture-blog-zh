# 使微服务对下游故障更有弹性

> 原文：<https://medium.com/geekculture/making-a-microservice-more-resilient-against-downstream-failure-247d81fd644f?source=collection_archive---------7----------------------->

![](img/ec899c4244db96914debf984938f15d8.png)

我很喜欢思考监控和[提醒](https://hceris.com/monitoring-alerts-that-dont-suck/)的问题。[过去几个月来，SLOs](https://hceris.com/multiwindow-multi-burn-rate-alerts-in-datadog/) 一直是我最近的困扰之一。

然而，如果不加以注意，所有这些指标都是没有价值的！比不值钱还糟。如果您的综合警报没有得到修复，您将会被接二连三的警报淹没。