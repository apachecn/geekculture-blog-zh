# iOS 应用程序状态

> 原文：<https://medium.com/geekculture/ios-application-states-part-3-31f9f6a4c5a8?source=collection_archive---------9----------------------->

## ui 应用程序状态转换和后台规则

![](img/6aa1495c5c11bacecae862ea116dad16.png)

最常见的用例是当用户点击应用程序图标，应用程序启动。
**1。)** App 唤醒
**2。)**你做一些初始化(可选，但几乎总是使用)
**3。)**创建一些 UI，屏幕
**4。)**触发一些 API 调用(可选，但经常使用)