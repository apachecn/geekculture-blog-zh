# 用 esbuild 构建一个库

> 原文：<https://medium.com/geekculture/build-a-library-with-esbuild-23235712f3c?source=collection_archive---------2----------------------->

## 如何用 esbuild 捆绑 ESM，IIFE 或者 CommonJS 库？

![](img/ed433ef0d5cbf69e6eb9b643b88971d9.png)

Photo by [Colin Watts](https://unsplash.com/@imagefactory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我最近开发了插件，并移植了 DeckDeckGo 的所有工具，用 T2 的 esbuild 来构建这些插件。

如果你想做同样的事情，希望这篇教程能帮助你开始！

# 介绍