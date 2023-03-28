# 使用 for_each 通过 Terraform for AWS 使用 Secrets Manager 创建多个 SSH 密钥

> 原文：<https://medium.com/geekculture/using-for-each-to-create-multiple-ssh-keys-with-terraform-for-aws-using-secrets-manager-2661af99405a?source=collection_archive---------4----------------------->

## 模块化| IaC |不可变| DevSecOps | Hashicorp | PEM

![](img/c50c39fb7bae7e50a52c343a5435deb9.png)

Photo by [Maria Cappelli](https://unsplash.com/@rikku72?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

因此，当涉及到与 AWS 上的 EC2 相关的秘密时，我遇到了一个名为“不要重复自己(DRY)”的干净代码问题。所以当我向前迈两步的时候，我会后退一步，以确保事情顺利完成…