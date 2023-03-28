# 构建分布式系统:API 失败

> 原文：<https://medium.com/geekculture/bulletproof-distributed-systems-how-to-fail-at-http-calls-e7f0b3240d19?source=collection_archive---------2----------------------->

## 以一个简单的客户端-服务器交互为例，探索所有可能出错的地方

![](img/d1b4a9e9c2f0cf4854232b94e522ad9e.png)

[pixabay.com](https://pixabay.com/cs/vectors/notebook-chyba-web-varov%c3%a1n%c3%ad-text-5906264/)

在本文中，我试图解释如何使用同步远程调用(HTTP，RPC 等)来集成软件。)，以及一个简单的**一行程序**远程调用如何需要大量的努力才能正确完成，这取决于…