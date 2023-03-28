# AWS IAM 权限界限

> 原文：<https://medium.com/geekculture/aws-iam-permissions-boundaries-781ca8c8e9c0?source=collection_archive---------1----------------------->

## 限制 IAM 实体的权限

![](img/d663d4f583b7f9811b19eb980123add2.png)

你好，世界！AWS 开发人员通常只需要访问少量的 AWS 服务来满足他们的项目需求。例如，web 应用程序开发人员可能只需要访问 AWS IAM、cloudwatch、S3、EC2 和 CodeCommit。授予对项目范围之外的其他服务的访问权限，会引入与权限相关的风险…