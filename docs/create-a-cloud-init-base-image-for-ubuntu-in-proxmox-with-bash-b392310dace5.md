# 用 Bash 在 Proxmox 中为 Ubuntu 创建一个 Cloud-Init 基础映像

> 原文：<https://medium.com/geekculture/create-a-cloud-init-base-image-for-ubuntu-in-proxmox-with-bash-b392310dace5?source=collection_archive---------7----------------------->

![](img/37314e27e079e0c3fff9737ee5642cd6.png)

Photo by [Gabriel Heinzer](https://unsplash.com/@6heinz3r?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 自动化| DevOps |虚拟化| SSH |农历 23

本文最初需要的是 Proxmox 上的 HAProxy 虚拟服务器。添加 HAProxy 服务器变成了寻找合适的 Linux 发行版(Ubuntu Lunar ),然后变成了为 Proxmox 创建一个虚拟机。然后最后来看这篇文章…