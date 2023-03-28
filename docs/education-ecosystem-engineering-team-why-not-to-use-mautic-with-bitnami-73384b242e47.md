# 教育生态系统工程团队:为什么不将 Mautic 与 Bitnami 一起使用

> 原文：<https://medium.com/geekculture/education-ecosystem-engineering-team-why-not-to-use-mautic-with-bitnami-73384b242e47?source=collection_archive---------11----------------------->

![](img/ceffbe2010e5678e767ba037ccfc2d37.png)

**简介**

Mautic 是面向所有人的营销自动化软件。它使营销和社区经理能够毫不费力地管理营销活动，通过电子邮件营销培养和吸引潜在客户，监控社交媒体，构建动态登录页面和表单，以及执行高级点滴营销活动。Mautic 是免费和开源的，可以通过 REST API 和 web-hooks 很容易地与第三方应用[集成。Mautic 旨在简化营销活动管理，包括电子邮件营销、社交媒体营销和活动报告的高级功能。](https://educationecosystem.com/education_ecosystem/2Pjrq-how-to-add-my-digitalocean-app-as-a-trusted-resource-for-my-managed-da)

安装 Bitnami 打包的 Mautic 是有问题的，正如本文所展示的——教育生态系统工程团队的案例研究。安装是错误的，设置 cron 作业变得非常困难。解决方案？直接安装 Mautic。

**比特纳米包装的 Mautic**

Bitnami 打包的 Mautic 为 Mautic 提供了一键安装解决方案。Bitnami 打包的 Mautic 是预先配置的，可以在任何云平台上立即使用，如 Google Cloud、AWS 或 Azure。

**为什么 Bitnami 包装的 Mautic 对你不好**

对于大型安装来说，安装这个软件包并不是一个明智的选择。随着时间的推移，Mautic 数据库快速增长，它保存了大量数据，并变得错误百出。这导致它随机崩溃。这也使得设置 cron 作业变得困难。Bitnami 也不支持这种安装。

**安装 Mautic 最好的方法是什么？**

直接安装 Mautic 是最好的方法。将它安装在您的云平台上很容易设置、管理数据库和设置 cron 作业。您将能够轻松设置您的营销活动，并整合您可能需要的任何第三方服务。在直接安装中，您可以设置数据库自动删除任何不必要的

**案例分析**

教育生态系统的工程团队在两年前安装了 Bitnami 打包的 Mautic。mautic 数据库在 6 个月内超过 200 GB，mautic 开始崩溃，分析无法加载。该团队随机得到 524 和 522 个错误。例如，当团队试图增加 ram 和 cpu 时，他们知道在 bitnami 设置中，不可能增加 RAM 和 CPU。

直接在 AWS EC2 实例上安装 Mautic 解决了使用 Bitnami 打包的 Mautic 进行安装时出现的问题。

**最终想法**

Mautic 是一款致力于营销自动化的开源软件。它允许企业自动化营销活动、线索生成、细分和联系/线索评分。安装 Bitnami 打包的 Mautic 被证明是有问题的，因为安装是错误的，很难建立 cron 作业，并随机崩溃。直接安装 Mautic 是一种可行的方法，如上面的[教育生态系统团队](https://educationecosystem.com/signup)的例子所示。