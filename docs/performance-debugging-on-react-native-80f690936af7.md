# React-Native 上的性能调试

> 原文：<https://medium.com/geekculture/performance-debugging-on-react-native-80f690936af7?source=collection_archive---------7----------------------->

您可以找到关于在构建 react 原生应用程序时如何解决性能问题的文章。你不会找到很多关于如何找到 react 原生应用中的瓶颈以及在哪里应用这些性能改进技术的文章。

就在上周，我正在开发一个在后台上传图片的反应性应用程序(另一篇文章也将在这方面发表，因为这很痛苦)。这个应用程序包含一个屏幕，列出了所有正在上传的图像及其进度。很快我们一次上传 100 张图片，我们注意到当…