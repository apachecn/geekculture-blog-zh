# 从 Terralith 到 Terraservice 与 Terraform

> 原文：<https://medium.com/geekculture/from-terralith-to-terraservice-with-terraform-acf990e65578?source=collection_archive---------8----------------------->

![](img/ec77c2c68952edb65a3e840f69964a36.png)

Photo by [Valery Fedotov](https://unsplash.com/@imlst?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是关于 Terragrunt 和 Terraspace 的三集系列的第一篇文章。

> 第一集:[从 Terralith 到 Terraservice 用 Terraform](/geekculture/from-terralith-to-terraservice-with-terraform-acf990e65578)
> 第二集: [Terragrunt 小抄](https://bit.ly/2YfWRED)
> 第三集: [Terraspace 小抄](http://bit.ly/3o2DrMV)

所有这些工具都可以用来将你的基础设施定义为一个试图应用 [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 模式的代码(IaaC)。

*先决条件:对主要地形概念的基本理解。*

![](img/4e61ef07de46c61d72f91451abc94993.png)

# 哪个是症状？

基础设施变得更加复杂，您需要
从这些需求中找出:

*   多地区，
*   多环境，
*   多账户。

**三“多”模式。**

当复杂性增加时，有多种方法来管理 IaaC。
管理多种模式的 Terraform 项目的发展由步骤、新功能和经验组成，可以让您兴奋不已。

就复杂性而言，一个典型的 terraform 项目通过这 5 种模式发展。既然我们想变得干巴巴[ [请看看这个伟大的演示文稿](https://www.youtube.com/watch?v=wgzgVm7Sqlk)，对它们进行深入的分析。

这里有一个简短的总结:

1.  **Terralith** :单一状态文件，单一定义文件，硬编码配置，本地状态。两个字:都在一起！
2.  **多 Terralith** :远程状态，多 terraform 文件，更好的利用变量→1:1 tfState-environment。
3.  **Terramod** :与 Env 变量互连的可重用模块，独立的 Env 管理→ 1:1 tfState-environment
4.  **Power(Terramod，n)** :嵌套模块(Base[infra]+系统特定)，专用模块 repo → 1:1 tfState-environment
5.  **陆地服务**:

*   独立管理逻辑组件:隔离并降低风险
*   多团队设置
*   分布式(远程状态):**N:1 = TF State:environment**
*   需要额外的编排工作
*   环境之间的存储库隔离

![](img/f8a22dacdb67ec10220a85f0eef16a47.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

现在，您至少有两种选择来管理“Terraservice”场景:

1.  花一些时间在一个**定制**脚本和管道上，解决所有的需求。
2.  开始学习 **Terragrunt** ， **Terraspace** 和**比较**它们。

> 我要解释的是第二个选项

让我们列出**关键需求**以及我们希望在 Terraservice 场景中解决的原因:

*   干——但我更喜欢权衡利弊。
    *有时会重复自己介绍的一些清晰的东西。*
*   客户、环境、区域和服务之间的组织结构。
*   同一个帐户中的多个状态文件，自动后端供应。与其拥有一个巨大的状态文件，不如分割它！
*   在同一帐户中管理多个地区。
*   团队之间同时工作。
    *当团队增加时，你需要增加并行度来减少冲突。*
*   在 TFvars 上分层(base.tfvars 与特定的 env staging.tfvars 合并)。
    *不重复所有变量。复制-粘贴可能是一个* ***严重的*** *问题！*
*   模块间的依赖“管理”。
    *维持模块间的低互连可以减少令人头疼的数量。*
*   在命令前后运行的钩子。
*   规划/应用所有内容的单一命令。
    *注意两个框架提供的“应用全部”命令。*
*   不会增加太多开销。
    *很少有人愿意为此学习一门新的编程语言。*

# 你觉得这个列表怎么样？

在下一篇文章中，我将用一个真实的例子展示如何用 Terragrunt 和 Terraspace 解决这些需求的主要部分。

![](img/5e942234f579c9081df2fc372dab8480.png)

> [关注我](https://pie-r.medium.com/)和[订阅](https://pie-r.medium.com/subscribe)来获取这个系列和下一个系列的更新！

![](img/ea0b13a7e23bac4817a9b62b3a1e9cb6.png)

Photo by [Omar Flores](https://unsplash.com/@__itsflores?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

参考资料:

[1][https://www.hashicorp.com/resources](https://www.hashicorp.com/resources)