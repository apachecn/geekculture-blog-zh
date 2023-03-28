# 游戏管理——单一模式和统一事件

> 原文：<https://medium.com/geekculture/game-managing-singleton-pattern-and-unity-events-d7c064bc4807?source=collection_archive---------14----------------------->

**目标**:了解单例设计模式和简单的 Unity 事件用法

## 游戏管理器 Singleton

在其他教程中，我们使用了*Manger 的概念，意思是一个由**管理**一些特定逻辑的脚本，这些逻辑不能被认为是游戏对象的“行为”。例如，我们有等级管理器，种子管理器，用户界面管理器等。

所谓 **singleton** 就是实例唯一的一个类。换句话说，你可以在每次运行时只实例化它一次