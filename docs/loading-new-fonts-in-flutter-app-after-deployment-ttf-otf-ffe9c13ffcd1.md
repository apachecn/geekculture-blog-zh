# 部署后在 Flutter 应用中加载新字体-。ttf，。光学传递函数

> 原文：<https://medium.com/geekculture/loading-new-fonts-in-flutter-app-after-deployment-ttf-otf-ffe9c13ffcd1?source=collection_archive---------17----------------------->

最近，我面临着一个奇怪但富有挑战性和乐趣的要求。我的经理希望我让用户能够从各种字体中进行选择。最初，我在资产中放置了三个字体文件，并将它们注册在 pubspec.yaml 中作为传统。这些字体将是默认字体。

现在，如果用户想尝试不属于 apk 的字体，他/她可以简单地从我们提供的字体列表中获得。字体在第一次被调用时被下载。作为开发人员，可以检查字体是否已经…