# 姜戈的会话和密钥

> 原文：<https://medium.com/geekculture/sessions-and-a-secret-key-in-django-9d7fa021f96c?source=collection_archive---------4----------------------->

在姜戈，有些事情就是有效的，你从来不用担心具体是如何有效的。其中之一就是会话。

您有多个用户访问您的网站。有些是登录的，有些只是客人。你只需要访问`request.user`来查看当前用户是否登录。但是在引擎盖下发生了什么？

## 会议

对于每个用户，都会创建一个会话。

会话只是数据库中名为`django_sessions`的表中的另一行，如下图所示: