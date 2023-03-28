# 在无服务器框架项目中本地使用 DynamoDB

> 原文：<https://medium.com/geekculture/using-dynamodb-locally-in-a-serverless-framework-project-92c996fcffdf?source=collection_archive---------5----------------------->

## 如何在无服务器框架项目中创建和访问用于本地开发的 DynamoDB 表——所有这些都不需要使用任何插件！

![](img/5ac6761089bd56f039e3260aa73fcbcb.png)

It’s literally a table. You try to come up with a better cover photo! Photo by [Hannah Busing](https://unsplash.com/@hannahbusing). Thank you, Hannah!

在本文中，我将演示一种将 DynamoDB Local(通过 Docker)与无服务器框架结合使用的方法。我们将创建一个引导脚本…