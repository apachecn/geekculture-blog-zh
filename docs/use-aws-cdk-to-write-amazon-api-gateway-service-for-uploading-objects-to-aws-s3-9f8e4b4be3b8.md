# 使用 AWS CDK 编写用于将对象上传到 AWS S3 的 Amazon API 网关服务

> 原文：<https://medium.com/geekculture/use-aws-cdk-to-write-amazon-api-gateway-service-for-uploading-objects-to-aws-s3-9f8e4b4be3b8?source=collection_archive---------3----------------------->

## 自动气象站 CDK + API 网关+自动气象站 Lambda +自动气象站 S3

![](img/162ebb769f83ffe13c5b5049a1bddc43.png)

AWS CDK (Source: [link](https://aws.amazon.com/blogs/devops/developing-application-patterns-cdk/))

在这篇文章中，我将带你完成部署包含 [AWS Lambda](https://aws.amazon.com/lambda/) 的堆栈的过程，该堆栈可用于获取 [AWS S3](https://aws.amazon.com/s3/) PUT/GET 操作的预签名 URL。这个 Lambda 将通过[亚马逊 API 网关](https://aws.amazon.com/api-gateway/)服务公开，并使用 [AWS](https://aws.amazon.com/cdk/) 进行部署…