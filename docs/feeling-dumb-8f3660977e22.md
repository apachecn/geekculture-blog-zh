# 算法之旅

> 原文：<https://medium.com/geekculture/feeling-dumb-8f3660977e22?source=collection_archive---------46----------------------->

我最近参加了拉胡尔·潘迪和 T2 的演讲，他们解释了为什么 DS(数据结构和算法)练习很重要，以及当你学习/练习算法时会有什么收获。(你可以在[的演讲中找到一些要点。我强烈建议在 LinkedIn 上与他们联系，注册他们的](https://www.linkedin.com/feed/update/urn:li:activity:6822313404636774400/)[空闲工作区](https://join.slack.com/t/techcareergrowth/shared_invite/zt-lt2tbjcn-LOAVIDuGPI~nkuc4woHDLg)和/或电子邮件群发。他们的陈述是绝对的黄金:谦逊/乐于助人/人性化。所以他们说的很多话引起了我的共鸣。我特别欣赏他们下面的图表。

![](img/1fc79d047fe8dc482227730010302d32.png)

The Standard Algo Journey as Described by A Couple Of True Algo Jedis Who Happen To Be The Nicest Guys

几个月来，我每天都在研究算法。当然，有些日子会比其他日子轻松。我没想到的是，一旦我开始觉得自己知道一点点自己在做什么，然后开始学习更复杂/要求更高的算法，我就会退回到“我觉得自己很笨”的状态。这种感觉令人担忧，但这个循环确实有意义:我们越是走出舒适区来挑战自己，我们就越是考验自己的技能……我们就越是怀疑自己的能力。

就我个人而言，这段旅程中我最欣赏的一点是我的熨斗朋友/同事，他们几乎每天早上都和我一起努力/延伸这段旅程。当我能够向朋友展示解决方案并帮助解释或澄清问题时，我感到非常兴奋，但我发现自己越来越经常地成为需要更多澄清的人。可以肯定的是，这是令人羞愧的。我提醒自己要有耐心，善待自己。(如果我自己这么说的话，在与他人共事时，耐心和友善通常是我的天性。)我们中的大多数人不会在几周或几个月的时间里变得“擅长算法”，但毫无疑问，继续努力是值得的。男孩，当你学习一个新的概念/想出一个可行的方法时，是令人振奋的，甚至是快乐的！考虑到这一点，我想带你们看一个我最近纠结过的算法。当我让它工作的时候，我几乎是在座位上跳舞！

这个问题叫做[人类可读时间](https://www.codewars.com/kata/52685f7382004e774f0001f7)(Codewars 上的 Kata/level 5)。*以下是说明和一些示例:

![](img/f3605c0f5dc845ab2a381571fb74d599.png)

Human Readable Time, Codewars

我的第一个想法是“我不知道该怎么做”。哦，那种“我觉得自己很笨”的情绪是很强大的！然后，我决定以某种方式迭代这个数字，以获得我的小时、分钟和数字。但是数字不像字符串或数组那样是可迭代的...我也知道 [padStart()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart) 会派上用场。(PadStart 允许我们在另一个字符串的开头添加/填充字符串字符，它与标准的字符串插值不同。PadStart 有两个参数:字符串的预期长度和要添加的字符，是“***”还是“00”还是“………………”)所以在关注这个问题一段时间后，我尝试了一些伪代码:

我最近看到/使用了除法(“/”)和模(“%”)组合的其他例子来分解数字。所以我决定在这里尝试一下:如果我们有 61 秒，把我们的秒分成这样:分钟= Math.floor(61 / 60)，秒= 61 % 60，我们将有一分钟和一秒钟的剩余时间。在测试数学概念时，我更喜欢使用非常简单的解决方案，以确保我的思维围绕着问题和准确的解决方案。我使用 Chrome 开发工具控制台和 MDN repls 来测试我的假设。

![](img/a7b9f88b5149082f62d8140e95232043.png)

testing assumptions with Chrome dev tools

![](img/202984580c4c9708c75395fa4e326a2e.png)

Thank you MDN for embedding repls in so many of your webpages!

我开始在我的代码和 console.logged 中放置一些变量。

请注意，我的假设有点偏离，以上。我的小时是准确的，但我的分钟实际上是返回秒。我只需要稍微调整一下数学。

我完成了我的变量，做了一些调整，继续测试我的假设，并得到了一些绿色支票！！！*嘿，现在有些道理了！！*这可能不是最性感的解决方案，但它是可读的，并且让我觉得自己很聪明。我的第一个形 5！

![](img/0597b2dcb1f207bb38fa056b6147732a.png)

Working Solution for Human Readable Time

*注意在使用之前，我必须将小时/分钟/更新秒转换为字符串。padStart()，因为在使用 padStart 时不能将数字强制转换为字符串。

![](img/006ab33d8214442be3174654ac0060b7.png)

This Green Response Makes Me So Happy

我强烈建议加入一个你信任的可以一起成功/失败的团队，如果可以的话每天练习。有一天我们都会成为算法绝地大师！！

![](img/a10a263e8e587e247f9c751f7e9acc25.png)

Consult the docs…. check your assumptions….

* [Codewars](http://codewars.com) 是一个开始学习算法的好地方。最简单的问题是 8 级的形，随着形数的减少，它们变得越来越有挑战性。最简单的 Codewars 问题比“简单”的 Leetcode 和二分搜索法问题要容易得多。在 Leetcode 和二分搜索法(以及其他代码挑战平台)上练习是很重要的，但是当你刚刚开始的时候，相信我，你会想要记录一堆“胜利”。