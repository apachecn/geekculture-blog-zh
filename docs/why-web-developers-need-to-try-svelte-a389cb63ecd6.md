# 为什么 Web 开发人员需要尝试苗条？

> 原文：<https://medium.com/geekculture/why-web-developers-need-to-try-svelte-a389cb63ecd6?source=collection_archive---------9----------------------->

Svelte 是当今众多新的 JavaScript 框架中的一个，还是更难忽略的一个？

![](img/8a2727112dc37fae7313cc22c4e9dee6.png)

您可能在某个时候听说过这个新的 JavaScript 框架。无论是 YouTube，你的开发者朋友，还是 Medium 上的文章，似乎都有很多关于 Svelte 的讨论。那么它是什么，为什么如此令人兴奋？

JavaScript 框架对这个行业来说并不新鲜，当然也不缺乏。然而，尽管可能有成百上千个框架，但大多数框架彼此之间很难区分，并且使用相似的方法和技术。它们也带来了各种各样的问题，比如膨胀和虚拟 DOM(这里有更多关于这个的内容)。

也就是说，Svelte 在一些非常酷的方面是不同的，并且将会更好地震撼这个行业。我已经使用它一段时间了，我有一些想法想分享一下它对行业的影响。更重要的是，我想谈谈你是否应该把它用于你自己的项目。让我们开始吧。

# 故事

故事是这样的,《纽约时报》的前图形编辑 Rich Harris 发现了当时 JavaScript 框架的一个缺陷，他们没有做出反应。我不想离题太久谈论反应性，但是你可以点击这个[链接](https://www.youtube.com/watch?v=AdNJ3fydeao)获得 Rich Harris 的深入解释。基本上，反应性是指如果两个组件的状态相互依赖，当一个组件更新时，另一个组件也应该知道要更新。

当其他 JavaScript 框架通过重新渲染子对象并使用虚拟 DOM 检查更改来处理状态更改时，Harris 开始着手构建一个真正反应式的 JavaScript 框架。因此，Svelte 诞生了，自 2016 年首次发布以来，其受欢迎程度显著增长。到目前为止，Svelte 被认为是最受欢迎的 JavaScript 框架，并且拥有最满意的开发者。

# 为什么它受人喜爱

很难让 89%的开发人员对某件事感到满意，但这正是 Svelte 设法做到的。通过易用性和强大的应用程序性能的结合，Svelte 提供了出色的开发人员体验。

## 易用性

在 React 这样的传统 JavaScript 框架中。构建组件是一个有些乏味的过程。下面是 React 中一个名为`HelloWorld.jsx`的组件的例子。

如你所见，React 基本上让你在你的 JavaScript 文件中写 HTML。一开始很好，因为这比自己做元素创建函数简单多了，但是这个组件这么简单，`HelloWorld.svelte`更简单。

简单地说，这个想法是让开发人员尽可能地接近编写 HTML、CSS 和 JavaScript。这是一个非常基本的例子，但是我将对 Svelte 和它的功能做一个全面的深入研究，我们将它与其他框架的特性进行比较。与竞争对手相比，它的简单性非常突出，这使得开发苗条的应用程序要快得多。

## 高性能代码

React、Angular、Vue 和几乎所有其他框架都使用虚拟 DOM 来呈现用户界面。每当应用程序的一部分状态改变时，虚拟 DOM 会遍历组件树，检查每个组件以查看其状态是否改变，以及是否应该重新呈现它(更多信息请参见[这里的](https://aidan-tilgner.medium.com/should-you-really-be-using-a-javascript-framework-fd2b4d37c28a))。

这个过程的问题是它很慢。然而，Svelte 没有虚拟 DOM，因此不会经历其他框架所面临的性能下降。你可能想知道这种表现是否真实，对此我会说是的。下面是它在多个测试中与 React、Vue 和 Angular 的比较:

![](img/c8b9a920ce3bc7cbd9f394be883d16a2.png)

如果你想亲自检查这些性能测试并进行比较，请点击[链接](https://krausest.github.io/js-framework-benchmark/2020/table_chrome_84.0.4147.89.html)。这里重要的一点是，通过抛弃虚拟 DOM 并拥抱真正的反应性，有大量的优化好处。苗条的应用程序有更多的性能提升空间，尤其是在较大的应用程序中。

## 那又怎样？

您可能听说过构建时间比运行时间更重要。基本上，你构建应用的速度比应用的整体性能更重要，至少在大多数情况下是这样。至于开发者为何如此钟爱 Svelte，我无法给出一个完全客观的原因，但我的猜测是因为 Svelte 让你两者兼得。如果你能在很短的时间内构建一个应用，并提高你的性能，这是一个双赢的局面。

# 这对行业意味着什么

## 乔布斯

因此，Svelte 是一个很酷的新框架，可能会使构建应用程序变得更有趣，但开发者也需要赚钱。因为这是一个相对较新的框架，苗条的工作实际上有点难以获得。根据 StackTrends 的数据，Svelte 在某一天只有大约 80 份工作列表。

如果你是一个身材苗条的开发人员，并想以此为职业，这是非常令人畏惧的，祝你好运找到一份合适的工作。也就是说，时间是站在斯维特一边的，80 份工作清单可能看起来不多，但就在去年，这个数字是 20。帖子已经增加，并且很可能继续呈指数增长，特别是当更多的开发者开始看到严重的好处。

React 在某一天有 26k 个列表，比它在 2013 年首次发布时高出一倍。也许这种受欢迎程度是由脸书(现在的 Meta)公司的支持和影响造成的，但我不排除苗条。值得注意的是，虽然 Svelte 最初是 Rich Harris 的个人项目，但他后来被 Next.js 的创始人 Vercel 聘用，全职从事 Svelte 的工作。

## 应用程序

随着框架的兴起，网站从主要的信息页面发展成为功能齐全的应用程序，拥有你所期望的原生应用程序的特性。随着 web assembly 的兴起，现在甚至可以通过将代码转换为 web 可以理解的高性能二进制文件来获得 web 应用程序的本机性能。

但并非所有领域的表现都有所提高。前端框架使构建应用程序变得更容易，并支持许多很酷的功能，但不幸的是，它们实际上可能会对你的网站的性能造成损害。然而，正如我们所看到的，Svelte 在性能方面已经是尽善尽美了，因为它基本上与编写在浏览器中本地运行的普通 JavaScript 相同。

以前框架在可伸缩性方面遇到性能问题，但这不会在将来阻止开发人员。这可能会导致行业采用过渡性的 Web 应用程序，它们的存在是为了弥合服务器端呈现和客户端呈现之间的差距。像 Svelte-Kit 这样的新的基于 Svelte 的元框架试图将这样的想法结合到像你我这样的开发者的新的开源解决方案中。

# 警告

## 属国

尽管 Svelte 很好，但在用它来建立你的下一个大的创业想法之前，有一些需要考虑的问题。其中最大的是普通的老支持。Svelte 是一个大的开源项目，但就目前的情况来看，Svelte 仍处于起步阶段。

如果想在 React 里做路由，很简单。你做`npm i react-router`然后把所有东西都包在一个容器里。不幸的是，到目前为止，Svelte 还没有一个精确的对等词，设置路由可能有点棘手。

这是因为虽然 Svelte 周围的社区很大，有很多贡献者，但它的规模远远不及 React。在 GitHub 上， [React 有 36k 分叉](https://github.com/facebook/react)，而 [Svelte 只有 2k](https://github.com/sveltejs/svelte) 。这并不是反对苗条，只是意味着它没有像 React 一样获得那么多的牵引力。然而，不幸的是，目前可供苗条开发者使用的第三方依赖和工具数量很少。

## 结构

尽管依赖关系不一定在 Svelte 的控制之下，但是框架本身却在。在使用 Svelte 之后，我确实发现了一些问题。比如错误，就有点难。一些错误消息非常具体，确切地告诉我我需要知道什么，而另一些则完全含糊不清。

最重要的是，当我在网上搜索这个错误时，很难找到一个好的例子，因为很少有开发者遇到同样的错误。这与 React 形成了鲜明的对比，如果我遇到一个错误，快速的谷歌搜索只需点击几下就能返回答案。

## 有关系吗？

至于这些元素是否真的会严重影响开发人员的体验，我不得不说不会。Svelte 的问题在新的框架中是意料之中的，过一段时间就会消失。例如路由可以使用 [Routify](https://www.routify.dev/) 来实现，它正在不断改进。错误通常是好的，我相信随着时间的推移，它们只会变得更好。

如果你因为这些小问题对使用 Svelte 有疑问，我会说无论如何都要学，或者等几个月直到它们消失。

# 结论

综上所述，Svelte 真的很酷。它速度快，重量轻，易于使用，唯一的问题是它是新的。我甚至可以说这是未来的趋势。新的和当前的框架已经从 Svelte 的成功中吸取了教训，这种情况不会很快改变。如果您想在您的前端开发技能上做一点面向未来的工作，请尝试 Svelte。

*大家编码快乐！*