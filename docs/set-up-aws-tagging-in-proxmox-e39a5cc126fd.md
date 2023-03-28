# 在 Proxmox 中设置 AWS 标记

> 原文：<https://medium.com/geekculture/set-up-aws-tagging-in-proxmox-e39a5cc126fd?source=collection_archive---------3----------------------->

![](img/8ee1a2410d76b075abdc7383c4aecbd5.png)

Photo by [Angèle Kamp](https://unsplash.com/@angelekamp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## AWS | DevOps |自动化| Proxmox |虚拟化|实验室

所以我想用 Ansible 建立动态库存，但不是 Proxmox。在 Amazon Web Services (AWS)中，标记是确保您可以静态识别虚拟机(ec2)的关键。本文将只讨论标记，而不讨论虚拟机(VM)的创建。如果你想学习如何创造…