# 使用 Netlify 实现跨多个 web 应用程序的无缝 Firebase 身份验证

> 原文：<https://medium.com/geekculture/enabling-seamless-firebase-authentication-across-multiple-web-apps-using-netlify-825cf836bd6b?source=collection_archive---------15----------------------->

![](img/9bc4ebc35f43e16861a68f8f8231c92a.png)

Photo by [Nicolás Flor](https://unsplash.com/@nicolasflorr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了给 [Reciprocal.dev](https://reciprocal.dev) 添加认证，我们一直在使用 Firebase，因为免费层给了我们足够多的钱来为 alpha 准备一个基本的应用程序，而无需支付一分钱。

Reciprocal.dev 由 3 个不同的网络应用组成；一个账户管理 app，一个用户旅程地图编辑 app 和一个用户旅程地图查看 app，然后我们…