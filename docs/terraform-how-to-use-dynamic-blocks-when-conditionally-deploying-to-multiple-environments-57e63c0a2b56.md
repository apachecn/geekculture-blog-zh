# Terraform:如何在有条件地部署到多个环境时使用动态块

> 原文：<https://medium.com/geekculture/terraform-how-to-use-dynamic-blocks-when-conditionally-deploying-to-multiple-environments-57e63c0a2b56?source=collection_archive---------0----------------------->

![](img/17f0c184d6c4ecee3a274591785e914d.png)

Photo by [Hello I’m Nik](https://unsplash.com/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/lego?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在之前的[出版物](/swlh/terraform-how-to-use-conditionals-for-dynamic-resources-creation-6a191e041857)中，我解释了如何在使用 Terraform 部署资源时实现条件逻辑。

根据 Terraform 文档，动态块*的行为很像 for 表达式，但它产生嵌套块，而不是复杂类型的值。
它在一个给定的复合体上迭代*