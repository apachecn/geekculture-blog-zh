# Google Maps 中图像的圆圈和大小调整的解决方案

> 原文：<https://medium.com/geekculture/the-solution-to-circled-and-resized-bitmapdescriptor-images-in-flutter-37ec0925b818?source=collection_archive---------6----------------------->

![](img/318530328fff7259d96e3a9ffc66d7d2.png)

Photo by [Patrick McManaman](https://unsplash.com/@patmcmanaman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是一个在应用程序中使用谷歌地图 SDK 的 Flutter 开发者，你可能会面临一个使用自定义图像的问题，尤其是在调整图像大小和裁剪图像时；我们将在本文中看到如何实现这一点。

如你所知，Google Maps SDK 没有使用本地 Flutter 小部件来显示标记图标，而是使用了位图描述符…