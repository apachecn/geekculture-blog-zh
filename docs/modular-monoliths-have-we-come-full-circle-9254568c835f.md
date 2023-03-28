# 模块化独石:我们回到原点了吗？

> 原文：<https://medium.com/geekculture/modular-monoliths-have-we-come-full-circle-9254568c835f?source=collection_archive---------40----------------------->

![](img/15c2ed4c1c0cb9c4d81d845eaaae0443.png)

# 首先是道歉…

这是对我朋友布兰登 [*@flybayer*](https://twitter.com/flybayer) 的公开道歉。回到 2020 年，当他发表 [Blitz](https://blitzjs.com/) 时，我回复了他的一条推文，大意如下:

> *“我不明白为什么人们想要回到 monolith，特别是 React 上面，我认为我们已经走过了那个时代，微服务+spa 是今天的规范”。*

我错了，大错特错…

# 有中间立场吗？

我最近一直在探索 JavaScript 生态系统中出现的许多新的很酷的东西，我注意到一个有趣的趋势:开发人员想要模块化系统的灵活性和可伸缩性，但没有它带来的维护和复杂性开销。

这就是像`[MicroLib](https://github.com/module-federation/MicroLib)`这样的后端工具和像`[Blitz](https://blitzjs.com/)`和`[Remix](https://remix.run/)`这样的全栈“元框架”承诺带回像 Ruby on Rails 这样的整体框架的“良好的旧生产力胜利”,但保持现代 web 的模块化和组件优先方法。

# 模块化整体

这个概念并不新鲜，可能也不太受欢迎；它是一种架构风格，通过在不同的域之间实施严格的界限并提高代码的可重用性，以模块化的方式构建应用程序，这使得代码组织和依赖性管理更加容易。模块化整体结构的关键是将组成系统的部件(模块)作为一个单一的部署单元，也称为“整体结构”。

# 为什么会有人用这个？

作为传统整体架构和成熟的微服务架构之间的中间地带，模块化整体架构在可扩展性、自主团队和独立交付方面仅带来有限的好处，然而，这可能是比从第一天开始使用微服务更好的方法。这遵循了 Martin Fowler 在他的文章[“Monolith First”](https://martinfowler.com/bliki/MonolithFirst.html)中的建议。

模块化整体可以作为一种识别系统边界的方式来实现，同时保持整体的灵活性和低维护开销，以便提高开发速度和更快的上市时间。

这一途径可能会导致系统的初始设计，并作为一种中间状态，可以在未来分割为单个微服务，但是，如果团队已经有了经验并对微服务感到满意，从第一天开始就有明确的系统边界，并且基础架构已经就绪，那么应该考虑直接使用微服务。

# 新一代模块化单片

像`blitz.js`(构建在`[next.js](https://nextjs.org/)`之上)这样的新的全栈“元框架”是如何成为新一代模块化整体架构的？

像`Blitz`这样的框架的工作方式是以单页面应用程序的方式保持前端独立，但连接到数据层，而不使用 REST/graph QL API，允许直接访问数据库。它仍然是一个单一的部署单元，但是，它有明确的界限，可以“剥离”并在未来转移到单独的 API，如微服务或无服务器功能。

# 六边形，到处都是六边形！

![](img/8fcf7a7568fc6322fa3ec68ad1f367fb.png)

如果你想让你的前端和后端分开，但你不想支付[“微服务溢价”](https://martinfowler.com/bliki/MicroservicePremium.html)，另一个有趣的模式出现了，就是像[“MicroLib”](https://github.com/module-federation/MicroLib)这样的库，构建在[模块联盟](https://webpack.js.org/concepts/module-federation/)之上，基于“六边形架构”来创建一个“polylith”，一个由多个(否则就是)微服务组成的整体。

与传统的“模块化整体”的关键区别在于，“多石”可以独立部署组成系统的多个模块。就可管理性、可重用性和自治性而言，这可能是“两全其美”的。

# 结论

高级工程师从建筑师那里学到了“视情况而定”这句话…那么你应该实现模块化的整体吗？这取决于您的需求以及您在应用程序生命周期中的位置。像 Shopify 这样的大公司已经成功地通过[实施模块化的整体架构](https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity)来扩展他们的整体架构，并证明答案并不总是微服务。