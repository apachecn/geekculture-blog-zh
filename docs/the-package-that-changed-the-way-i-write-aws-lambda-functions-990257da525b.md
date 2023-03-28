# 这个包改变了我编写 AWS Lambda 函数的方式

> 原文：<https://medium.com/geekculture/the-package-that-changed-the-way-i-write-aws-lambda-functions-990257da525b?source=collection_archive---------1----------------------->

## 如何在几分钟内解决 AWS Lambda 层和依赖关系——再也不用碰 zip 文件了！

![](img/9750389873413f3e74a2760482bef2c9.png)

Photo by [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

AWS Lambda 是一个非常有用的服务。它允许以无服务器的方式快速运行小代码片段。然而，Lambda 总是为如何管理依赖关系而斗争。