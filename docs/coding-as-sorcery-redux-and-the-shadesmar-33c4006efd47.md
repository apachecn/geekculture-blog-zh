# 作为巫术的编码:Redux 和 Shadesmar

> 原文：<https://medium.com/geekculture/coding-as-sorcery-redux-and-the-shadesmar-33c4006efd47?source=collection_archive---------18----------------------->

如果编码是巫术(确实如此)，那么编码中的不同概念与来自幻想小说、神话和黑暗艺术历史的各种魔法系统相比如何？这就是本系列探讨的问题。(下面是零件[一个](/codex/oop-magic-systems-and-the-allegory-of-the-cave-bf891c4cdf0e)和[两个](/geekculture/coding-is-sorcery-potions-object-composition-in-javascript-245310cfb405)。)

这一次，我们将深入研究最广泛使用的前端库之一， [Redux](https://redux.js.org/) ，以及奇幻小说中最精确和最有思想的魔法系统制作者之一的最新产品，布兰登·桑德森的 [*风暴之光档案*](https://en.wikipedia.org/wiki/The_Stormlight_Archive) *。*(前方轻微剧透。预先警告！)

![](img/64dd798063bf0ede8daf5fade0ff2fde.png)

Szeth and Shallan from “*Stormlight,” a composite of* [*two artworks*](https://bluehighwind.blogspot.com/2014/04/the-stormlight-archive-words-of-radiance.html) *by* [*Michael Whelan*](https://www.michaelwhelan.com/) *(*[*via*](https://wallpaperaccess.com/stormlight-archive)*)*

# Redux 是什么？

Redux 是 Javascript 应用程序的一个可预测的状态管理工具，本质上是一个控制应用程序数据流和从应用程序的一部分到另一部分访问最新数据的架构。在软件工程中，“状态”是指以前事件和用户交互的记忆信息。例如，当您在输入字段中键入注释时，每一次击键都会改变应用程序组件的状态，从而更新该组件，使最新状态反映您最后一次键入的所有内容。当您键入评论时，只有您正在键入的表单需要记录您的草稿状态。但是一旦你发布了它，应用程序的其他部分需要知道你这样做了。这就是国家管理的用武之地。

Redux 特别受欢迎与 [React.js](https://reactjs.org/) 一起使用——以至于 Redux 团队发布了一个官方的 [React-Redux](https://react-redux.js.org/) 库。我开始自学如何使用 Redux，同时致力于编写 bootcamp 的最终项目，boot camp 是一个名为旁注的社交注释应用程序。([在 Github 上查看](https://github.com/alecmagnet/marginalia))。)下面是一个例子，说明为什么我需要一些东西来管理我的状态:

假设你发表了一篇评论，解释了一首诗或一个短篇故事中的一些短语。呈现这首诗的组件将您的评论发送到后端，存储在数据库中，并更新页面以包含您的评论。恶心！但是当你看到你的评论时，你会想，“天哪，我真的需要更新我的个人资料照片了。这是我在一级防范禁闭期间拍的，这些天我看上去整洁多了。”因此，您导航到配置文件编辑页面。您不希望重新加载整个应用程序，并再次获取每个组件的所有数据。那会让事情变得很慢。但是你的个人资料页面怎么知道你发布了那个评论并把它添加到预览和链接列表中呢？这就是 Redux 的作用。Redux 将您需要在整个应用程序中可用的任何状态保存在任何组件都可以访问的对象中。

那么，除了它们惊人相似的标志，Redux 与风暴之光档案馆和它的超自然领域沙德斯马有什么关系呢？

![](img/f8bd7a5329a4655b2d2338db5d7996be.png)![](img/15dd407fee146c7596e7126ebf7eb4d8.png)

[React, Redux](https://regroove.ca/blog/react-redux/), & [Stormlight](https://www.brandonsanderson.com/alternate-stormlight-symbol-reveal/)

# 什么是 Shadesmar？

布兰登·桑德森精心制作了他的魔术系统。(他最好是这样——没有什么比这更适合这位首先提出根据魔法系统的严格和定义来区分“硬”和“软”幻想的作家了。这听起来可能有点令人讨厌的区别，但事实上它非常聪明，因为根据桑德森的说法，两者都可以以不同的方式支持好的故事。不过，那是以后的博客文章了。)

在计划出版的十部系列小说中，只有前四部 *Stormlight* 已经出版。但是——加上几部较小的中篇小说——这个系列到目前为止已经有 3000 到 4000 页。在这篇文章中，我们甚至无法总结桑德森错综复杂的世界构建和魔法系统，所以我将只关注几个方面。即是说，**剧透警报！我将要描述的一些东西需要几千页才能揭示出来。继续下去，后果自负。**

暴风之光的世界分为三个领域，物理、精神和认知。这最后一个，也被称为 Shadesmar，连接着另外两个。

![](img/c8dc75911e57e1dbed5cbea22b425759.png)

Shadesmar Map by [Isaac Stewart](https://coppermind.net/wiki/Isaac_Stewart) (via [Fandom](https://stormlightarchive.fandom.com/wiki/Shadesmar))

某些人(我猜还有实体)能够在这些领域之间旅行，或者至少在物理领域和阴影世界之间旅行，特别是[超自然生物](https://stormlightarchive.fandom.com/wiki/Surgebinding)，或者能够驾驭自然和魔法世界力量的人。在认知领域中，存在于物质领域中的每一件事物都是一种反映——也许是一种认知模型，或者类似于精神灵魂的东西。一些[超能力者](https://stormlightarchive.fandom.com/wiki/Soulcasting)可以访问甚至改变这些模型——一种被称为灵魂铸造的力量——这种力量改变了它们在现实世界中相应的复制品。

# 他们之间有什么类比？

如果所有这些听起来很复杂，那是因为它确实很复杂。话说回来，Redux 也是。事实上，redux 的实现非常棘手，当我在 Giphy 上搜索“redux react”时，首先出现的是这个(它告诉了你很多东西——既告诉了你 Redux 有多令人沮丧，也告诉了你当你让它工作时它有多有用):

也就是说，Redux 在概念上并不难理解，它提供了一些方便的类比来帮助解释 *Stormlight* 的魔力。事实上，Shadesmar 的一个想法是通过与 Redux 的普遍状态对象进行类比。在你的网站的用户界面背后——物理世界——是一个认知领域，存储着应用程序中每个组件的所有共享状态。它在你的 UI 后面，但还是前端。(我认为在这个类比中，你的后端应该是精神领域。桑德森还没有在他的作品中走得那么远，但我怀疑当他这样做的时候，这个比喻将开始崩溃。)

使用 Redux，您可以访问一线界面背后的这种认知领域。你可以调用 hooks——React 和 Redux 的 surges 版本——来查找那个领域的信息(`useSelector`)并改变它(`useDispatch`),从而改变 DOM 上显示的内容，DOM 是一个相当于物理领域的 web 应用程序。

# 反思和变形

这篇文章实际上标志着我的一个重要转折点。在本系列中，我的编码知识第一次更有助于解释这个神奇的系统，而不是相反。

在某种程度上，这是桑德森的魔术系统是多么复杂和错综复杂的产物。但我想在这里停下来，思考一下这种变化反映的另一个现实——事实上，在这 15 周的训练营中，我已经了解了足够多的编码知识，我的大脑会反射性地跳到代码库中进行类比，以帮助解释棘手的概念。明天，我就要毕业了，这段经历真正改变了我的认知领域，将我的大脑重塑成一名程序员。

不过，我会继续这个系列。一如既往，请让我知道你对这篇文章主题的看法。我喜欢听你的想法！

![](img/f7a7484f320d15ecfe56c24948d409c4.png)

Fanart of Pattern and Shallan in Shadesmar by [Gondalier](https://www.deviantart.com/gondalier/art/Shadesmar-494029111)