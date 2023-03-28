# 360 IT Check #21 — Kotlin 秋季路线图、机器学习的新 EC2 实例等等！

> 原文：<https://medium.com/geekculture/360-it-check-21-kotlin-autumn-roadmap-new-ec2-instances-for-machine-learning-and-more-211b99e87446?source=collection_archive---------32----------------------->

![](img/7a8d3dc16a08f5a16c59a2c3525d06e7.png)

# 科特林秋季路线图的亮点

JetBrains，Kotlin 编程语言 T1 背后的公司，分享了他们秋季路线图 T3 中的一些 T2 亮点。

## 新编译器前端

编译器是“科特林的心脏”。这是用这种语言编写的每个应用程序的贡献所在。目前，重点是把新的前端带到足够好的状态，让公众预览。在它达到这个状态之前，团队很自豪地分享，构建速度平均快了一倍。

你可能会期待新的 K2/JVM 编译器预览版在 2022 年春天发布。

请点击 YouTrack 上的[点击这里](https://youtrack.jetbrains.com/issue/KT-46756)。

## 科特林多平台移动电话

Kotlin devs 又有了一个热门的新解决方案。目前，Kotlin 多平台移动(KMM)并不处于(非常)稳定的状态，它将于 2022 年春季进行[测试。](https://blog.jetbrains.com/kotlin/2021/10/kmm-beta-roadmap-video-highlights/)

随着新的(实验性的)[kot Lin/本机内存管理器](https://blog.jetbrains.com/kotlin/2021/08/try-the-new-kotlin-native-memory-manager-development-preview/)，并发性也应该得到改善。这个解决方案的计划也是为了使它更加稳定。

相关票: [#1](https://youtrack.jetbrains.com/issue/KT-49525) 和 [#2](https://youtrack.jetbrains.com/issue/KT-49520) 。

## 名称空间

名称空间是许多语言中都有的语言特性，比如 C#、C++、PHP 和 TypeScript。

该团队正在开发一个解决方案的原型，该解决方案允许“提高 Kotlin 与 Java 静态方法的互操作性，并将支持任何 Java 类型上的扩展。”

相关路线图票[此处](https://youtrack.jetbrains.com/issue/KT-11968?_gl=1*13zrdzm*_ga*ODQyNTk4NDYzLjE2MTIzNzY3ODE.*_ga_J6T75801PF*MTYzNjM1OTAzNC4yOTguMS4xNjM2MzU5MTI5LjA.&_ga=2.124001795.881152214.1636359035-842598463.1612376781)。

## Kotlin 测试覆盖工具

Kover 是一个测量 Kotlin 代码覆盖率的新工具。它适用于所有的语言结构。该工具将基于开发人员的反馈进行开发，这就是为什么该团队鼓励每个人都尝试一下。

相关的路线图代号是[这里是](https://youtrack.jetbrains.com/issue/KT-49527)。

## do kka——生成图书馆文档的新工具

Dokka 是库开发人员的另一个工具。该团队希望开发人员对所有文档都有统一的体验——无论是官方的还是第三方的。这就是为什么外观和感觉会跨上下文统一，以减少认知压力。

相关路线图[门票](https://youtrack.jetbrains.com/issue/KT-48998)。

## 额外:

作为补充说明，Kotlin 团队最近发布了一个视频，介绍了发布该语言新版本的过程。

# Docker 桌面 4.2

## 暂停和继续

在你的计算机上运行 Docker 在性能和能源使用方面是很昂贵的。该工具背后的团队意识到了这一点，并特别为所有笔记本电脑用户准备了一个至关重要的特性。你现在可以暂停 Docker 桌面。容器的当前状态将保存在内存中。

## 对更新机制的更改

全球计算机用户的“最爱”功能——持续不断的更新弹出窗口，现在已经不复存在了。关于 Docker 新版本的通知现在以简洁方便的徽章形式出现在图标和上下文菜单中。

随之而来的是禁用自动检查更新的能力。适用于所有 Docker 订阅层，现在，您可以完全控制何时更新您的应用程序。

后台的下载也会占用你太多的带宽，而且似乎是在最糟糕的时候开始的。如果你想下载新的更新，只需在设置中取消“总是下载更新”选项。

# 采用 NVIDIA GPUs 的新 AWS EC2 实例

EC2 G4 实例是一种让每个人都能获得“经济高效”的 GPU 能力来进行机器学习和要求苛刻的图形应用的方式。

11 月 11 日，AWS 宣布了全新的 G5 实例，拥有多达八个[英伟达 A10G](https://www.nvidia.com/en-us/data-center/products/a10-gpu/) 张量核心 GPU，以及第二代 AMD EPYC CPU。新款 G5s 的“性价比”比 G4s 高 40%。

NVIDIA 和 AMD 支持的 EC2 实例面向媒体和娱乐公司、远程工作站、机器和深度学习应用等。你可以在 EC2s 上运行 Linux 和 Windows。

点击查看详情和完整公告[。](https://aws.amazon.com/blogs/aws/new-ec2-instances-g5-with-nvidia-a10g-tensor-core-gpus/)

*最初发表于*[*https://www.itmagination.com*](https://www.itmagination.com/blog/360deg-it-check-21-kotlin-aws-nvidia-amd-docker)*。*