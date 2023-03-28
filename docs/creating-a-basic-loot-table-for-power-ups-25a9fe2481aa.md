# 创建一个基本的道具表

> 原文：<https://medium.com/geekculture/creating-a-basic-loot-table-for-power-ups-25a9fe2481aa?source=collection_archive---------16----------------------->

与其依赖随机产生的均等机会，不如给产生的机会增加一些权重。

![](img/476e3cbc3c346259f1410482bb232de9.png)

**今天的目标:**创造一个加权的战利品产卵器来代替完全随机的战利品产卵器。

## 我们开始吧！

不仅仅需要一个游戏对象来产卵，现在每次加电都需要一个数值来表示它的产卵机会。

创建一个新的*脚本。把它从“类”改成“结构”，更容易用作变量。在该结构中，创建另一个结构，如下所示:*

*![](img/49d8329538e5f4d8ec53a2eb23b88226.png)*

*Add “[System.Serializable]” to both Structs to make them serialized for Inspector modification*

*![](img/2eca3a6e68be0071d1bf7d3437c6381b.png)*

*If you want, add a range attribute with a suitable maximum to “Chance”, to make the Inspector look nicer.*

*在"***【loot table】***"内添加一个"***【loot table object】***类型的数组。*

*![](img/fdcc7126b389a1a4392c492d7485eb10.png)*

*The **LootList** won’t change in-game, so an Array is better than a List.*

*在"***【loot table】***内部，创建一个函数返回数组中所有产卵机会的总和。*

*![](img/a66e4eb517eacc5dc1c00e2af3a34dfa.png)*

*现在创建一个函数从数组中返回一个随机的 ***游戏对象*** ，基于重生几率权重。*

*![](img/65e92a15a7045698ae2ee693e22b4481.png)*

*Two vital variables inside the function*

*为该函数添加一个 ***ForEach*** 循环。*

*![](img/68565917c2349366070b9f182696b298.png)*

*最后，在函数的末尾加上一个错误日志，万一出了什么问题，**这应该是不可能的**。*

*![](img/2a7ac1610a7d542b67954ed0c1e63af9.png)*

*现在，我使用一个名为 SpawnManager 的脚本来管理所有的生成工作。在那个脚本中，我做了一个变量" ***_powerups*** "，类型为"*"。**

**![](img/3154daa5e377ec2376373656217b650a.png)**

**Because they are serialized, they structs appear in the Inspector to fill**

**当我想制造能量的时候，我只需要使用" ***_powerups "。getrandomweightedsbownable()***”。**

**![](img/f3013ffd20ee08dafeb045efb9879b15.png)**

**Spawn a power-up based on all weights**

**![](img/7a51e6dadb973bbc22f4d529a168f356.png)**

**Make sure to fill the array in the Inspector, and that at least one Chance is above zero.**