# 向 Django 项目添加实时通知

> 原文：<https://medium.com/geekculture/add-real-time-notifications-to-your-django-project-6ee2aed38597?source=collection_archive---------2----------------------->

对 Web 套接字的另一种看法。

![](img/0f640ff9f4bdcc13200bc268c090463b.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

另一天另一篇科技文章。在上一篇文章中，我回顾了 WebSockets 的基本介绍，但在这篇文章中，我将回顾 WebSockets 的一个用例，它实时生成通知。所以不用再等了，让我们开始吧。

## 1)为您的应用程序设置 ASGI