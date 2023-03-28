# 有效的 Java:同步访问共享的可变数据

> 原文：<https://medium.com/geekculture/effective-java-synchronize-access-to-shared-mutable-data-d43de894699?source=collection_archive---------10----------------------->

![](img/5ef39264c1b2d2da880a718b8195d947.png)

Photo by [Vincent Branciforti](https://unsplash.com/@vfbranciforti?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这个项目开始了一个新的部分 *Effective Java* ,它关注于并发性以及围绕这个主题的技巧和诀窍。第一个主题关注同步的使用。当 Java 开发人员想到同步时，他们可能会想到 *synchronized* 关键字。这是有充分理由的，这个关键字非常有用。Java 开发人员对*同步*的主要使用之一…