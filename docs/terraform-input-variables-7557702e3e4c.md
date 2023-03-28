# Terraform 输入变量

> 原文：<https://medium.com/geekculture/terraform-input-variables-7557702e3e4c?source=collection_archive---------0----------------------->

## 动态传入 Terraform 中的值

![](img/8cda7153421b0fd29caed4a71b330914.png)

在我以前的 Terraform 文章中，我们的代码中有硬编码的值。如果我们希望在创建和修改基础设施时动态传入一些值，该怎么办？

比如在代码中定义提供者时使用变量而不是硬编码的访问键，还是由创建基础设施的用户来决定…