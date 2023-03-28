# 如何在你的机器上轻松设置 Yocto 项目

> 原文：<https://medium.com/geekculture/preparation-and-set-up-of-yocto-project-on-your-machine-369689e9e81d?source=collection_archive---------1----------------------->

# 准备

安装必要的 Linux 操作系统软件包。

这取决于您使用的 Linux 发行版。为了简单起见，您应该使用 Yocto 项目指南中推荐的 [Linux 发行版。](https://www.yoctoproject.org/docs/1.8/yocto-project-qs/yocto-project-qs.html)

我准备的例子适用于 Linux Ubuntu 和 Debian。

```
$ sudo apt-get update
$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib
$ sudo apt-get install build-essential chrpath socat cpio python python3
$…
```