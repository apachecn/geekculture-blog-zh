# Django 教程:模板标签与上下文处理器

> 原文：<https://medium.com/geekculture/django-tutorial-template-tags-vs-context-processors-3775f6fed339?source=collection_archive---------20----------------------->

我从事个人项目已经有一段时间了，在构建过程中偶然发现了一个有趣的点。为了将事情放入上下文中，我想要构建一个站点页脚来保存站点内容的附加信息。可能大多数人都知道，页脚部分是在整个应用程序中共享的，不管你在哪个页面。因此，在这篇文章中，我将介绍两种不同的方式来建立这样的功能。

![](img/8be0c0ef891e57dab7ab156a1d747a5f.png)

Photo by [Faisal](https://unsplash.com/@faisaldada?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)