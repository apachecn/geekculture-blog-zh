# 将 Figma 插件与 Esbuild 捆绑在一起

> 原文：<https://medium.com/geekculture/bundling-figma-plugin-with-esbuild-e6d41e28f872?source=collection_archive---------19----------------------->

## 用速度极快的 JavaScript bundler es build 为 Figma 构建一个插件。

![](img/c102d181666c324a90e8861052fb16e2.png)

Photo by [Uillian Vargas](https://unsplash.com/@vargasuillian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/fast?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我最近发布了一个新的开源插件[来导出](https://www.figma.com/community/plugin/950777256486678678/Figma-to-DeckDeckGo)[图马](https://www.figma.com/)帧到 [DeckDeckGo](https://deckdeckgo.com) 幻灯片。

因为我喜欢从我的经验中受益，学习和尝试新的概念，而不是使用 Figma 文档中描述的捆扎机，所以我决定给出一个…