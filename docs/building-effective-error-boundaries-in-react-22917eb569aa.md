# 在 React 中建立有效的错误边界。

> 原文：<https://medium.com/geekculture/building-effective-error-boundaries-in-react-22917eb569aa?source=collection_archive---------10----------------------->

![](img/33e9e9657791c9d8f3d79fc3b08f970c.png)

Photo by [Matt Hudson](https://unsplash.com/@lalunecreative?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

React 16 中引入了错误边界，作为捕捉错误的工具，否则这些错误会导致整个应用程序崩溃。我们可以将这些错误封装在一个后备 UI 中，为用户提供一些有用的反馈，告诉用户可能出现了什么问题，或者至少提供一种方法来解决当前的错误，比如页面重载或其他地方的链接。