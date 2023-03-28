# 有效的 Java！明智地返还期权

> 原文：<https://medium.com/geekculture/effective-java-return-optionals-judiciously-719b28110cb0?source=collection_archive---------47----------------------->

![](img/b2693e1dbd49a9f50ba3037049c619f3.png)

Photo by [Nubelson Fernandes](https://unsplash.com/@nublson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Java 8 之前，如果一个方法不想在某些时候返回值，而在其他时候返回值，有几种选择。该方法可以返回一个`null`，该方法可以抛出一个异常，或者您可以提出一些可以返回的状态保存对象(尽管我从未见过这种情况)。前两个选项都不太好。返回`nulls`只是要求空指针异常…