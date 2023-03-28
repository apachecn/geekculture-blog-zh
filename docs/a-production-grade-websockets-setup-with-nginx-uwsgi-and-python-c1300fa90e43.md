# 一个使用 Nginx、uWSGI 和 Python 的生产级 Websockets 设置

> 原文：<https://medium.com/geekculture/a-production-grade-websockets-setup-with-nginx-uwsgi-and-python-c1300fa90e43?source=collection_archive---------12----------------------->

![](img/1024449abf1704a0f1e66771117d60c4.png)

在本文中，我将简要介绍如何设置一个高性能且简单的 web sockets 后端服务器，以用于 Python (Flask)应用程序。

这与我们在 [Mixed CRM](https://mixedcrm.com/) 中使用的设置相同，用于支持我们的实时更新和通知。

[**混合 CRM**](https://mixedcrm.com) 是一款简单的自动化/物业预订和销售 CRM，让您轻松……