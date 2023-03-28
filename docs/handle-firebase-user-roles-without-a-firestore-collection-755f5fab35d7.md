# 无需 Firestore 集合即可处理 Firebase 用户角色

> 原文：<https://medium.com/geekculture/handle-firebase-user-roles-without-a-firestore-collection-755f5fab35d7?source=collection_archive---------1----------------------->

![](img/073a4f0f16d5dee235bd1721f7639a92.png)

# 介绍

Firebase 是在应用程序中处理用户认证的一个很好的方法。如果您需要存储大量用户信息，如联系信息或用户活动，那么您可以在 Firestore 中创建用户集合，但我经常看到开发人员创建用户集合来存储像用户角色这样简单的东西。这是不必要的，而且…