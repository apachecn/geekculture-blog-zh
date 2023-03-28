# 使用 Amplify Lambda 函数调配并发

> 原文：<https://medium.com/geekculture/provisioned-concurrency-with-amplify-lambda-function-8540f5836401?source=collection_archive---------17----------------------->

![](img/e68346e9de98d028f17f6e700cd3c88b.png)

供应并发是保持 Lambda 功能正常运行的一个特性。尽管 Amplify 不支持这种开箱即用的特性，但我们可以通过定制一个 CloudFormation 文件来自动生成供应的并发性。在这篇文章中，我分享了一些技巧来解决针对供应并发的特定问题。例如:

*   如何设置预配的…