# 如何使用自动气象站 CDK 为自动气象站 Lambda 保暖

> 原文：<https://medium.com/geekculture/how-to-keep-aws-lambda-warm-using-aws-cdk-a6bf2b9cface?source=collection_archive---------6----------------------->

## 了解如何定期触发 AWS Lambda 以避免冷启动

![](img/73574be8a201eafde234d968fad870db.png)

在这篇短文中，我们将学习如何通过定期触发来保持 AWS Lambda 的温暖。我们将创建一个事件规则来每分钟触发一次 Lambda。保持 Lambda 温度将避免冷启动，并大幅减少响应时间。