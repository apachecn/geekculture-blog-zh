# 摘要:在 OCI 的 Windows 和 Linux 虚拟机上安装 Oracle Database 19c 和 OBIEE 12c

> 原文：<https://medium.com/geekculture/summary-installing-oracle-database-19c-and-obiee-12c-on-windows-and-linux-vms-in-oci-24ec3283777d?source=collection_archive---------26----------------------->

![](img/da27b47a950045ca7c4f175267acff2d.png)

如果你在过去的一个月里一直关注我的 LinkedIn，你可能已经注意到我一直在分享我写的一些教程博客帖子。对许多人来说，这似乎是随机的。让我给出一些背景。

我最近被甲骨文聘为云解决方案工程师，在数据分析和人工智能支柱部门从事售前工作。简而言之，当潜在客户强烈考虑选择 OCI 作为他们业务的云提供商时，他们就会来找我的团队。我的团队有责任向潜在客户展示什么是可能的，如果他们采取下一步行动并决定选择 OCI 作为云提供商会是什么样。我们通过在云中展示演示来实现这一点。在交易签署并完成后，我们与客户的关系并没有就此停止。它还在继续。在这一过程中的每一步，我的团队都会为客户提供支持，特别是当涉及到我们所说的与转换云提供商和/或将 OCI 与其内部计算机服务器和/或其他云提供商集成相关的“成长的烦恼”时。

我目前仍在接受培训，正在学习 Oracle 云的细节。我的经理希望我们欣赏 OCI，并彻底了解为什么它比内部部署的计算机服务器更易于使用，并且在几乎所有情况下都更好，但是还有什么比亲身体验为什么内部部署的计算机服务器如此麻烦更好的方式来了解云的优势呢？

这是一个绝妙的练习—在 OCI 创建一些虚拟机，将这些虚拟机视为本地服务器，并在虚拟机上安装 Oracle Database 19c 和 OBIEE 12c。听起来很简单，对吧？

不…一点也不。做这些的文档呢？少之又少。我相信从我开始这段旅程到现在已经差不多一个月了…我现在刚刚完成这些装置。这是我写这些博客的动机。我想确保没有人会不必要地花一个月的时间来做这些安装。我写了博客记录了整个过程，从开始到结束。每个博客都非常详细，包括大量的图片。几乎任何人都应该能够跟随他们。我将所有这些链接整合成一篇最终的博客文章。我希望这最后一篇博客能成为您的主页，如果您需要在本地服务器上安装 Oracle Database 19c 和/或 OBIEE 12c，您可以参考这篇博客。

## 这里是我的 Linux 博客的链接

![](img/2586720eda3990d3cafdbb5927b2aae5.png)

*   步骤 1: [在 OCI 创建一个 Linux 虚拟机，并在 NoMachine 中远程访问它的 GUI](https://jaredbach.io/creating-a-linux-vm-in-oci-and-remotely-accessing-its-gui-in-nomachine-3e82794447b2)
*   步骤 2: [在 Linux 上安装 Oracle Database 19c](/geekculture/oracle-database-19c-installation-on-linux-e184dde4ce03)
*   步骤 3: [在 Linux 虚拟机上安装和配置 OBIEE 12c](/geekculture/install-and-configure-obiee-12c-on-linux-vm-2f31f49ea07c)

## 以下是我的 Windows 博客的链接

![](img/32ff479a7b5feb6e9a8d6d0ced91530e.png)

*   步骤 1: [在 OCI 创建一个 Windows 虚拟机，并在微软远程桌面中远程访问它的 GUI](https://jaredbach.io/creating-a-windows-vm-in-oci-and-remotely-accessing-its-gui-in-microsoft-remote-desktop-a20d555b9a0)
*   步骤 2: [在 Windows 上安装 Oracle Database 19c](/geekculture/oracle-database-19c-installation-on-windows-5a0561843fbc)
*   步骤 3: [在 Windows 虚拟机上安装和配置 OBIEE 12c](https://jaredbach.io/install-and-configure-obiee-12c-on-windows-vm-1c5487ddbaeb)

我希望如果你读了我的任何博客，你会发现它们很有帮助，并且它们会节省你的时间和压力。如果您有任何反馈、问题或只想联系，请在评论中告诉我您的想法，或在 [LinkedIn](https://linkedin.com/in/jaredibach) 或 [Twitter](https://twitter.com/jaredbach_io) 上给我发 DM。干杯！