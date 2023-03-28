# 什么是 Arch 用户存储库？

> 原文：<https://medium.com/geekculture/what-is-the-arch-user-repository-dd2bd98a3df2?source=collection_archive---------10----------------------->

## 如何使用 AUR 下载和分发软件？

![](img/1efc558ace796bd4a7a338206f27b8a0.png)

Wallpaper from [www.pixelstalk.net](http://www.pixelstalk.net)

新的 Arch Linux 用户可能已经使用过 **pacman** 来安装来自标准 Arch Linux 库的软件。然而，有时您可能需要安装标准 Arch 存储库中没有的软件。您正在寻找的软件很可能位于 Arch 用户存储库中。

下面是如何使用 Arch 用户存储库在 Arch Linux 操作系统中下载和分发软件。

## 什么是 AUR？

AUR 代表拱形用户库。这是一个用于 Arch Linux 系统的免费开源软件库。

AUR 和标准的 Arch Linux 存储库之间的区别在于，任何用户都可以将他们的软件提交到存储库中。这个关键特性使得 AUR 成为最大的 Linux 软件仓库之一。另一个区别是，这个存储库允许您将源代码直接提交到存储库中。Arch Linux 是一个基于二进制的发行版，这意味着程序将作为二进制文件安装，而不是从源代码安装。[但是，您可以使用 Arch 用户存储库和 Arch 构建系统将其转化为基于源代码的发行版。](https://wiki.archlinux.org/title/Arch_Build_System)

## 如何从 AUR 软件仓库安装软件？

![](img/9ad054bf6f4d034538f518a22745ad56.png)

Photo by [Christin Hume](https://unsplash.com/@christinhumephoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

您可以手动或自动安装 AUR 软件包。手动方式在一定程度上会遵循这种结构。

```
git clone [https://aur.archlinux.org/[software_name].git](https://aur.archlinux.org/[software_name].git)
cd [software_name]
makepkg -si
```

然而，大多数人会使用一个自动化的包构建器来下载、编译和安装他们选择的软件。这些 AUR 助手之一是一个名为 [**yay**](https://github.com/Jguer/yay) 的命令行程序。该程序与 **pacman** 软件包安装程序具有相同的标志和选项。标准 Arch 存储库中不存在该包。因此，您必须运行以下命令来安装这个包。

**来源**

```
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

**二进制**

```
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```

一旦你完成了程序的安装，你可以运行下面的命令来开始使用 **pacman** 助手程序。

```
yay -S [pkg_name]
```

如果你想使用臃肿的图形编辑器来安装 Arch 用户库。例如，在 Manjaro 中有一个叫做 **pamac 的图形化包管理器。**默认情况下，GUI 程序将只安装标准的 Arch Linux 程序，但是，您可以更改配置以启用 AUR。

[](https://archived.forum.manjaro.org/t/enable-aur-in-pamac-from-the-terminal/97900/6) [## 从终端启用 Pamac 中的 AUR

### Short:我只是想知道是否有一个命令可以在 pamac 上启用 AUR，或者是否至少有一个文件可以让我更改…

archived.forum.manjaro.org](https://archived.forum.manjaro.org/t/enable-aur-in-pamac-from-the-terminal/97900/6) 

## 如何向 AUR 提交代码？

任何人都可以将代码提交到 AUR，但是，您需要一个帐户才能将代码提交到 AUR。任何基于 Linux 的程序都可以发送到 AUR，但是，它们需要一些修改。

*   构建文件:这告诉你如何编译和安装软件
*   PGP 密钥:验证安装在您系统上的代码的有效性

一旦你有了，创建一个帐户，并通过网络发送包裹。

 [## AUR(英国)-首页

### 在“包详细信息”页面的“包操作”框中可以提交三种类型的请求:孤立…

aur.archlinux.org](https://aur.archlinux.org/) 

## AUR 检查病毒和恶意软件吗？

![](img/1920740bdafac77c0202fa73370dd519.png)

Photo by [Michael Geiger](https://unsplash.com/@jackson_893?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

AUR 是一个开放的宝库。因此恶意软件可以被发送到存储库中。如果恶意行为者试图危害一个被用作许多软件的依赖项的软件，这尤其是一个有害的问题。

不幸的是，AUR 的服务器上没有运行恶意软件扫描程序。但是，存储库由社区检查。恶意软件将被报告并从存储库中删除。因此，不能保证所有的包都没有恶意软件。建议您手动检查安装的每个软件的差异，或者运行某种辅助自动扫描。这对于没有经验的用户来说是不可能的，因此建议他们不要使用 AUR。

风险仍然比 Windows 低得多，因为除了您之外，没有人必须在 Windows 操作系统上执行恶意软件检查。

您可以阅读更多有关如何减少从 AUR 安装恶意软件的信息。

[](https://unix.stackexchange.com/questions/456417/how-to-check-an-aur-package-for-malicious-code) [## 如何检查恶意代码的 AUR 包？

### Unix & Linux Stack Exchange 是一个问答网站，面向 Linux、FreeBSD 和其他类似 Un*x 的操作系统的用户

unix.stackexchange.com](https://unix.stackexchange.com/questions/456417/how-to-check-an-aur-package-for-malicious-code) 

## 如何创建自己的 Arch 存储库？

您可以创建自己的存储库。您可能出于几个原因想要这样做。拥有一个存储库对于计算机组织管理来说是非常好的。一个原因可能是你想用特定的使用标志或代码修改来编译软件。然而，您可以将它存储在您的存储库中，而不是为您拥有的每台机器重新编译它。然后配置每台机器从您的存储库中安装软件包。

这里有一个关于如何创建自己的 Arch 存储库的指南。

[](https://www.arcolinuxiso.com/how-to-create-your-own-online-arch-linux-repository-on-github-and-use-it-on-any-arcolinux/) [## 如何在 github 上创建自己的在线 Arch Linux 存储库，并在任何 ArcoLinux 上使用它

### 然后我们克隆我们新创建的 github。git 克隆 https://github.com/arcolinuxteaching/teaching_repo 现在我们创建…

www.arcolinuxiso.com](https://www.arcolinuxiso.com/how-to-create-your-own-online-arch-linux-repository-on-github-and-use-it-on-any-arcolinux/) 

## 关于 Arch 用户存储库的深入指南

本指南只是对 AUR 的概述。如果您想将您的软件或其他软件添加到库中，我建议您阅读 AUR 上的完整 wiki。

 [## Arch 用户存储库

### Arch 用户库(AUR)是一个社区驱动的 Arch 用户库。它包含包描述(…

wiki.archlinux.org](https://wiki.archlinux.org/title/Arch_User_Repository) 

IT 和工程领域是快速发展的领域。跟不上意味着你将被落在后面。跟上的最好方法是保持最新的新闻和教育内容。[订阅免费电子邮件列表，将您的职业生涯提升 10 倍。](/subscribe/@dretechtips)

**加入我们吧，因为 50 多位想要快速提升职业生涯和知识基础的人已经注册了。**

**相关内容**

*   [如何在 Linux 和 BSD 中实现任务自动化？](/geekculture/how-to-automate-tasks-in-linux-bsd-91d0b0560f5)
*   [快速简单的安装 Arch Linux 的方法](/geekculture/the-quick-and-easy-way-to-install-arch-linux-70b9bfc35863)
*   [为什么 Windows 比 Linux 好？](/@drechang/why-windows-is-better-than-linux-da410b8d9689)