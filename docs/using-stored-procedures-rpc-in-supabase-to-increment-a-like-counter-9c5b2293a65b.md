# 在 Supabase 中使用存储过程(RPC)来递增“Like”计数器

> 原文：<https://medium.com/geekculture/using-stored-procedures-rpc-in-supabase-to-increment-a-like-counter-9c5b2293a65b?source=collection_archive---------21----------------------->

![](img/b24591878c717d0aeb8d870eaa1c2abf.png)

[Photo by Joshua Reddekopp](https://joshuaryanphoto.com/)

在我最近的兼职项目 [Lila](http://lila.mytinyai.com/) 中，我想要一种简单的方法来为网站上的报价增加一个“喜欢”计数器。当我使用[Supabase——开源 Firebase 替代品](https://supabase.io/)作为我的后端数据库时，我想也许在 API 中有一个增量函数。

我真的不想读取数据库，更新我的应用程序中的计数，然后写…