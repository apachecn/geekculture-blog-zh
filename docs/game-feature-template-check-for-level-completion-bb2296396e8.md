# 游戏特征模板:检查关卡完成情况

> 原文：<https://medium.com/geekculture/game-feature-template-check-for-level-completion-bb2296396e8?source=collection_archive---------11----------------------->

**目标**:检查自己是否能通过下一关

这种逻辑是有弹性的，可扩展的，可以应用于几种游戏类型:你只需要**重新校准**要检查的**条件**。

在本例中，我有一个具有物品收集/购买功能的动作游戏，我的条件是:

> 要赢得关卡并进入下一关，你必须拿到最后一扇门的钥匙