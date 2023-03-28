# 从 AWS Lambda 函数有效地运行 R 脚本

> 原文：<https://medium.com/geekculture/effectively-running-r-scripts-from-aws-lambda-functions-6af85caa1571?source=collection_archive---------1----------------------->

学习使用 AWS Lambda 和容器映像运行 R 脚本

![](img/0ac0fb583440824dae78ebd4eb66ffe7.png)

Photo by [Nubelson Fernandes](https://unsplash.com/@nublson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

AWS Lambda 通过[运行时](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-concepts.html#gettingstarted-concepts-runtime)支持多种语言。在撰写本文时，AWS Lambda 支持的运行时有:Node.js、Python、Java、C#、Ruby、Go 和 PowerShell。

这给通常使用 R 作为主要开发工具的开发人员或数据科学家带来了困难…