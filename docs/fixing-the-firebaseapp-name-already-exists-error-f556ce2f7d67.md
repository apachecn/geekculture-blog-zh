# 正在修复“FirebaseApp 名称已经存在！”错误

> 原文：<https://medium.com/geekculture/fixing-the-firebaseapp-name-already-exists-error-f556ce2f7d67?source=collection_archive---------12----------------------->

![](img/cfe0966d1a31c1288cb029471d3e9dc7.png)

[https://unsplash.com/@xps](https://unsplash.com/@xps)

老实说，我不知道为什么这没有内置在 Firebase 中，甚至没有记录在 Firebase 中。但是，如果您开发了一个带有热重新加载的 Firebase 应用程序——每次保存时——您的代码将再次尝试重新初始化 Firebase，导致以下错误；

```
FirebaseApp name [DEFAULT] already exists!
```