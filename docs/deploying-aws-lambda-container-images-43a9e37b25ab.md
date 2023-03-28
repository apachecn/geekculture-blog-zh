# 部署 AWS Lambda 容器映像

> 原文：<https://medium.com/geekculture/deploying-aws-lambda-container-images-43a9e37b25ab?source=collection_archive---------6----------------------->

了解如何使用 AWS CDK 和 Python 将 AWS Lambda 函数打包为容器映像。

![](img/9b9e0e55c489cde431edbdaaf057bcd7.png)

Web illustrations by [Storyset](https://storyset.com/web)

AWS Lambda 是 AWS 最广为人知的服务之一。根据 AWS 网站的说法，这是一种计算服务，让你无需配置或管理服务器就能运行代码。你只需要在需要的时候运行你的代码，它会自动从每天几个请求扩展到每秒几千个请求。