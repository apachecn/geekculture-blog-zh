# 用 Atop 查找常见的 Linux 服务器问题

> 原文：<https://medium.com/geekculture/find-common-linux-server-problems-with-atop-6b613e02905e?source=collection_archive---------8----------------------->

![](img/a8f2ae0dacc20a675368baed9502b1d1.png)

Photo by [Marc PEZIN](https://unsplash.com/@fennings?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/servers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大家以前都用过`top`。这是一种快速调查行为不端的 Linux 主机的可靠方法。你输入三个字母，按回车键，你会看到一个快速、简单的列表，列出了当前占用基本系统资源的内容。但是当`top`不够用的时候会发生什么呢？

*短暂的 CPU 峰值怎么办？*