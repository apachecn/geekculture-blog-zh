# Django 的 WebSockets 初学者指南

> 原文：<https://medium.com/geekculture/a-beginners-guide-to-websockets-in-django-e45e68c68a71?source=collection_archive---------1----------------------->

有时你需要“一直”保持联系

![](img/ed20f256e833fddaf9f0078c29112503.png)

Photo by [Chirayu Trivedi](https://unsplash.com/@rc820?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

HTTP 是好的，我的意思是我们在 Django 上创建的 API 和视图运行在 HTTP 上，但是和所有的东西一样，HTTP 也有一些缺点，这使得它不是需要更多“实时”元素的情况下的理想解决方案。

为了理解我刚才所说的，我们必须先看看 HTTP 是如何工作的，为什么，正如我所说的…