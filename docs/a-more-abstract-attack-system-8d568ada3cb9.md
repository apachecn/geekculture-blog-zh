# 更抽象的攻击系统

> 原文：<https://medium.com/geekculture/a-more-abstract-attack-system-8d568ada3cb9?source=collection_archive---------16----------------------->

**目标**:重构攻击系统，使之模块化

在这里，我将向你展示我如何**重构**攻击系统，使其更加抽象，更加模块化，对玩家和敌人都适用。

让我们首先假设玩家和敌人**共享同一个游戏对象** **结构**。

主对象，携带几个组件，如脚本和碰撞器来检测点击，子游戏对象携带 sprite 渲染器及其子对象，对象携带 hitbox 碰撞器。后者将提供一个共同的 hitbox 脚本…