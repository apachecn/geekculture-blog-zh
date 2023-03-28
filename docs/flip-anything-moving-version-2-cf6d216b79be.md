# 翻转任何移动的物体—版本 2

> 原文：<https://medium.com/geekculture/flip-anything-moving-version-2-cf6d216b79be?source=collection_archive---------24----------------------->

目标:以更专业的方式，在玩家和敌人移动时翻转他们

在之前的[文章](/geekculture/1-line-of-code-and-your-2d-player-will-look-where-its-going-8c1f4d1991a5)中，我写了在移动时翻转播放器精灵的可能性，只使用一行简单的代码，战略性地放在 StateMachineBehaviour 中。如果你通读了那篇文章，你会像我一样感受到**简单的力量。但是如果你再去一次，你肯定会注意到**是多么的僵硬**，依赖于玩家特定的组件，而**不能被其他游戏对象重用。****