# 为什么 Create React App 在 2022 年就过时了

> 原文：<https://medium.com/geekculture/why-create-react-app-is-outdated-in-2022-efd4cb8b2207?source=collection_archive---------1----------------------->

## 即使你是第一次使用 React，你也不应该使用 Create React App

![](img/a638a0ca5eff7d64b3d5379e7f7fd5e7.png)

Photo by [Fili Santillán](https://unsplash.com/es/@filisantillan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

C reate React App 是几乎每个开发人员在学习 Javascript 框架 React(包括我在内)时首先学会使用的，我认为这导致了一些重大缺陷。

# 1.创建 React 应用程序将整个应用程序构建到一个文件中

虽然这对初学者来说非常方便，但这会导致开发过程中的问题，

*   较慢的装载速度
*   无法进行生产构建
*   如果不喜欢默认配置，很难配置

Create React 应用程序使用客户端渲染(CSR)来捆绑 Javascript 文件，并构建一个文件来在客户端上渲染所有文件。这些都是在客户端浏览器中完成的，因为在默认配置中，React 无法访问服务器端渲染(SSR)。Create React App 抽象了项目内部的所有东西和正在使用的单个依赖项`react-scripts` ,这对于初学者来说可能很神奇，少了一件需要担心的事情！但是现在作为一个初学者，你可能不知道 transpiler(babel)或 bundler(web pack)是什么，你甚至可能不知道这些是什么(它们是 Javascript web 开发周期中非常重要的部分)！！

因此，从猖獗的抽象来看，处理 Create React 应用程序中的大量过度简化可能会导致理解 Javascript 的重要部分出现一些问题。我喜欢考虑为前端 web 开发创建 React 应用程序，就像 Python 对 C 一样。Python 抽象了 C 中许多最大的学习点，如指针和内存分配，而不是为用户处理几乎所有事情的较慢的解释语言。

![](img/04137a525852332f26ff78a62990d8a9.png)

Photo by [Clint Patterson](https://unsplash.com/@cbpsc1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 2.创建 React 应用程序的竞争

众所周知，前端框架空间已经饱和，似乎每六个月就有一个新的框架或新的语言问世，新的时尚占据了这个空间。学习新的语言和跟上这些新技术更难。这迫使大公司在构建新软件时明智地选择依赖关系，随着新的 React 框架不断涌入，他们坚持使用非常一致的 LTS 框架，这些框架运行良好，而 Create React App 不在其中。

React 已经成为最受欢迎的前端开源框架之一，并被脸书、优步、Airbnb、Shopify、Pinterest 和网飞等公司使用。虽然大多数时候这些大公司使用的框架是为他们的需求和规范定制的，但一些最好的开源项目仍然可供使用。

![](img/8a01aff2b365e13988a90532f81d710c.png)

Photo by [Florian Krumm](https://unsplash.com/@floriankrumm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 3.Next.js

Next.js 是由 Vercel 制作的生产级开发包，它为您的所有开发需求添加了非常有用的渲染选项，如我们之前讨论的经典 CSR、SSR 和许多混合选项，以帮助满足任何其他需求。在 Next.js 中有最小的抽象，允许您直接查看和修改现有的流程。包括一个 API 和 pages 部分，使开发变得快速、简单，并且(大部分)易于遵循。如果初学者要问从什么框架开始最好，我会推荐 Next.js 作为最好的下一步。

你可以在他们的[网站](https://nextjs.org/)和[演示](https://nextjs.org/examples)上更深入地了解他们提供的东西。

# 4.盖茨比（姓）

Gatsby 是由 Gatsby 的优秀人员制作的另一个生产级开发包，它允许快速和卓越的加载时间，以及内置的 GraphQL 和一个充满插件的社区，允许您将任何 CMS 与您的网站连接，无论是 Shopify、WordPress 还是 HubSpot。内置的 GraphQL 支持非常适合在您的网站中构建简单的图表并跟踪交流。

如果你不知道 GraphQL 为什么这么棒，读一下[我关于它的文章](/@collinpfeifer/is-graphql-the-future-of-http-requests-f68067cde2f1)。

据报道，与竞争对手相比，Gatsby 的加载时间快 50%，构建时间快 20 倍。这一切都是通过在静态 HTML、CSS 和 Javascript 包中构建他们的站点来实现的，以随着时间的推移减少负载并提高性能。这也绝不允许网站失败，即使依赖关系随着时间的推移而改变，因为它们仍然是建立在网络的基本构件之上的。

要了解更多关于他们框架的信息，请查看他们的[网站](https://www.gatsbyjs.com/)和[演示](https://www.gatsbyjs.com/starters/)以更好地了解他们提供了什么。

# 结论

我非常喜欢使用 React 的所有版本，从创建 React App 开始，到像 Next.js 和 Gatbsy 这样更大的项目，我对开发的热爱一直没有改变。如果你是初学者，我建议你跳过 Create React 应用程序的加载和学习阶段，学习 Gatsby 和 Next.js 的诀窍。你自己学习的经验是无价的。