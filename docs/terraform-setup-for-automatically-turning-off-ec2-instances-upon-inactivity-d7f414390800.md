# Terraform 设置为在不活动时自动关闭 EC2 实例

> 原文：<https://medium.com/geekculture/terraform-setup-for-automatically-turning-off-ec2-instances-upon-inactivity-d7f414390800?source=collection_archive---------5----------------------->

![](img/b7ad9da8d250b77de370d66ea1c0b2a9.png)

当 EC2 实例没有实际使用时，关闭它们可以帮助您或您的公司显著降低成本。例如，如果您在 EC2 实例上运行 Tableau 服务器，那么只要在某个时间段内没有活动会话，您就可以关闭它。

**模块**