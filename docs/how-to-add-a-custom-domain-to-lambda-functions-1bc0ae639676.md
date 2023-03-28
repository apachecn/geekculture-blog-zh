# 如何向 lambda 函数添加自定义域

> 原文：<https://medium.com/geekculture/how-to-add-a-custom-domain-to-lambda-functions-1bc0ae639676?source=collection_archive---------8----------------------->

![](img/0c85f8dc0ad7df79a15e4ef1890cf6b0.png)

Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/name-tag?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如何向 lambda 函数添加自定义域的分步指南

**我们为什么需要自定义域名？**

当您使用 Amazon API Gateway 为 Lambda 函数部署带有 HTTP 端点的 web API 时，它会给出一个很长的 URL，如下所示

> https://apiID.execute-api.awsregionID.amazonaws.com/apiSta…