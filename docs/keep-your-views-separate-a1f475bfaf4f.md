# 保持你的观点独立

> 原文：<https://medium.com/geekculture/keep-your-views-separate-a1f475bfaf4f?source=collection_archive---------16----------------------->

![](img/d1e17daed245e5cc72563ce1f13022b4.png)

Photo by [**Markus Spiske**](https://www.pexels.com/@markusspiske?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/one-black-chess-piece-separated-from-red-pawn-chess-pieces-1679618/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

当我们有两个看起来相似的视图时，我们通常会想到创建一个可重用的视图，比如

"*我将创建一个* [*UIView*](https://developer.apple.com/documentation/uikit/uiview) *，然后在我的两个控制器中使用该 UIView 解决问题*"

你可能会因为在这里考虑了可重用性而感到自豪，因为为什么不呢。(面向对象的氛围)

这可能在少数情况下有效，但我讨厌打破它" ***这不是一个明智的举动我的朋友*** "

通过在两个地方重用同一个视图，我们现在可能已经解决了这个问题，但是迟早如果客户添加一个新的特性说

![](img/1297c56047270692dc4e0ab67af85c31.png)

Photo by [**Kaboompics .com**](https://www.pexels.com/@kaboompics?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/happy-coffee-6347/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

## *嘿，你记得这两个相同的视图，现在我希望其中一个有一个按钮，说咖啡和咖啡将有自己的工作流程*

很好，现在对于许多开发人员来说，这可能看起来像是一个小变化，解决这个问题的第一直觉是

> 我们可以通过添加一个 if 条件来添加按钮，如果用户在一个特定的视图控制器上，我们会做一些约束管理，我们应该很好地去这个需求是没有问题的

完美现在你已经交付了客户想要的东西，但是你知道需求是如何永远不会被冻结的，并且变更需求还会继续出现🙄

几天前，客户又回来说

![](img/213168051b476e763e6ba75b375eb714.png)

Photo by [**Afta Putta Gunawan**](https://www.pexels.com/@apgpotr?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/assorted-decors-with-brown-rack-inside-store-683039/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

## *嘿，你还记得我们有咖啡按钮的那个视图吗，是的，所以我想改变它的标题颜色，并在它上面显示一些折扣券，如果用户接近我的任何咖啡供应商的话。*

现在我们正在考虑如何做到这一点，我们已经为按钮做了小的约束管理，让我们为标题颜色做同样的事情。

> 因此，我们决定添加一些 if 语句并进行更改，为折扣券添加 UI 元素，并为它们添加约束管理，完善策略我们仍然是可重用性俱乐部的成员，因为我们喜欢可重用性和面向对象。

几天后，客户又来了，说

## *嘿，你还记得我们有咖啡按钮并改变了标题颜色的那个视图吗，现在我想添加两个包含用户名和用户电子邮件的文本字段，并将它们存储在我们的后端*

现在，如果你还在考虑添加一个 if 条件，然后在这里做一些约束管理魔术，那么我的朋友，我们正朝着错误的方向前进。

我们可以清楚地看到这个屏幕是多么的不稳定，以及客户要求对它进行更改的频率。

拥有一个 UIView 并在多个地方使用它们是明智之举

# 当视图完全不变并保持原样时，当我说“照原样”时，我的意思是没有 ui 或任何业务逻辑变化。

如果改变是改变颜色或使按钮半径更圆，确定不需要为这些创建单独的视图，但一定要问客户

> “嘿，无论我们在哪里使用此屏幕，都可以看到相同的更改，您仍想继续这些新的更改吗？”

当客户对任何一个外观相似的视图进行修改时，应该会在你的脑海中响起多个警报声，这表明现在是你应该为它创建一个单独的 UIView 的时候了，因为这个屏幕有可能会发生变化。

读完以上内容后，你可能会想

> *“啊，这家伙在说什么，一个小的约束管理，如果条件永远不会伤害任何人，毕竟我们是面向对象城镇可重用性俱乐部的高级会员”*

像这样的小变化会导致大问题，如果你很快放弃这些小变化，你也会放弃更大更难看的代码变化。

然后在 6-12 个月后，当你再次访问代码库时，你会想，到底是谁写了这样的代码，并且会为在 git 历史上看到你的名字而感到自豪(曾经有过这样的经历)

如果客户要求改变一个外观相似的屏幕，那么它就有可能改变，而这些改变很少会从您过去的屏幕中引入新的业务逻辑。

因此，与其编写多个 if 语句，或编写约束管理代码或任何其他创造性工作，不如将它们分开是明智之举。

[关注点分离](https://en.wikipedia.org/wiki/Separation_of_concerns)很久以前就出现了，这是一个非常酷的概念，每个希望改进的程序员都必须知道。

我希望这篇文章是有帮助的，告诉我你是如何管理这样的需求的。