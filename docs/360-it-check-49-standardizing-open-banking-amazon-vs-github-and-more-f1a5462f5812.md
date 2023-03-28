# 360 IT Check #49 —标准化开放式银行、Amazon vs GitHub 等等！

> 原文：<https://medium.com/geekculture/360-it-check-49-standardizing-open-banking-amazon-vs-github-and-more-f1a5462f5812?source=collection_archive---------14----------------------->

![](img/64c9b35cce240fa059a99cdd78f7456a.png)

七月四日。批准《独立宣言》的纪念日。在这一天，我们祝愿所有来自美国的朋友和伙伴一切顺利。

![](img/0bc0df1051b2bac739ced4bb5feda8b1.png)

# 全球开放银行标准化

金融科技领域的新工具经常出现。它们为那些厌倦了银行传统产品的人带来了变革之风。它们在世界上有很强的立足点，我们必须每天使用它们。

这就是所有工具尽可能紧密集成的原因。的确如此，尽管重要的问题是整合财务数据的法律途径。“墨西哥有[……]金融科技法，澳大利亚有消费者数据权，巴西有开放式银行，等等。”一些监管者已经建立了一种友好的方式来告知开发者他们的法律义务——比如英国的[开放银行标准门户](https://standards.openbanking.org.uk/)。德国的法规也很容易获得，尽管是 PDF 格式的。

然而，世界各地的不同方法阻碍了公司的创新和可能性。

**底线**

在当今的全球化世界，一个管理开放银行业的国际监管机构将极大地帮助所有金融科技公司，这些公司往往希望在默认情况下开展全球业务。关键是要让法律法规变得容易理解，让所有人都能理解。

目标是让公司创造出默认安全的伟大产品；向用户提供价值并保护客户数据安全的产品。

观看下面关于开放式银行的演讲:

# 亚马逊对 GitHub 耳语。密语者 vs 副驾驶。

当 GPT-3，开放人工智能的语言生成器，写了一篇文章向我们保证它没有“[消灭人类](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)的计划，”它还在其他各种文章中提出了开放人工智能在未来可以帮助我们的所有方式。其中一个方法是人工智能如何开始从工程师那里接管一些编码工作，同时还侮辱我们的智慧。

去年，微软在预览版中推出了 GitHub Copilot ，这是一款通过在 Microsoft Visual Studio、Noevim、JetBrains 和 GitHub Codespaces 等程序中生成代码建议来帮助编码人员更快完成项目的工具。开发者可以将它与 Python、Javascript、Typescript、Go、Ruby 等编码语言一起使用。

亚马逊也不甘示弱，现在已经发布了关于他们自己的开放式人工智能辅助编码工具 CodeWhisperer 的消息。该公司表示，这不是微软工具的翻版，而是他们已经工作了一段时间的东西，几年前与 DevOpsGuru 和 CodeGuru 一起奠定了基础。

CodeWhisper 将主要允许与 Java、JavaScript 和 Python 一起使用，并将与 Visual Studio 代码、JetBrains、AWS Cloud9、AWS Lambda 和各种 ide 集成。

该工具允许您通过分析您的代码注释来接收代码片段，并确定哪些云服务和公共代码库适合该任务。然后，它会直接在您的代码编辑器中提供建议。

有趣的是，这个消息恰好在 GitHub 宣布副驾驶正式上市的时候发布。该工具现在每月 10 美元或每年 100 美元。

**底线**

GitHub 工具的有效性还没有定论。虽然它加快了创建代码的时间，但一项研究表明，超过 40%的代码包含可能给最终产品带来安全风险的错误。

一项[的研究还发现](https://arxiv.org/pdf/2108.09293.pdf)“结果表明，尽管 Copilot 通过增加代码行来衡量提高了生产率，但由于在随后的试验中删除了更多的代码行，产生的代码质量较差”

可悲的是，GitHub Copilot 可能会引入一些安全漏洞，或者引入在错误许可证下授权的代码。亚马逊已经考虑到了这些教训，因为该公司强调他们的工具还将为 Java 和 Python 提供安全扫描，帮助检测他们项目中的漏洞。它还将包括一个跟踪器，以确定推荐的代码是否在训练数据中使用，允许开发人员查找和审查代码样本。

这两家大公司之间的战斗现在已经进入了人工智能辅助编码领域，增加了科技巨头们正在工作的[友敌空间的不断增长的列表](https://www.wsj.com/articles/tech-giants-cooperate-while-competing-frenemies-for-life-11617293819)。

[最后，我们想强调的是，这些引擎是为了帮助开发者，而不是取代他们。](https://www.itmagination.com/blog/rise-ai-code-autocompletion-engines-github-copilot-tabnine-kite)

# 一个全新的网络框架

我们知道你在想什么:“另一个 JavaScript 框架？”在任何一天，这都是真的。这一次，有点不一样。

来自 Deno 创造者的一个新框架在这里，它是稳定的。Fresh 应该是 Deno 对臃肿的现代网站以及 Next.js、Nuxt.js、Sveltekit 等网站的回应。创作者如何试图解决这个问题？**默认情况下，服务器不会发送任何 JavaScript 给用户**；如果我们需要一些，我们将需要在一个特殊的文件夹中创建组件。与 [Next.js](https://www.itmagination.com/blog/next-js-12-release) 或 [Nuxt.js](https://www.itmagination.com/blog/nuxtjs-3) 不同，该框架不需要构建步骤。你写下你的代码，然后…它就准备好了。

当你确实需要一些 JS 时，框架使用 Preact 使事情变得非常简单。它也使用了您已经习惯的与 [React](https://www.itmagination.com/blog/react-18-what-changes-does-it-bring) 相同的 API。

**底线**

Deno 没有一个引人注目的生态系统原生框架。Aleph.js 是一个选择，尽管它在雷达下飞行。新鲜可能会改变这一点。

可悲的是，对于 Deno 来说，JS 全栈框架的其他组件(比如 ORM)并不存在。Node 的兼容层是有的，尽管它并不完美。

观看 Fireship 关于框架的视频:

# 尤雨溪和之间的对峙有什么反应

Vue 的创建者和主维护者尤雨溪似乎对 React.js 有个人恩怨，最近他推荐 [Vite 的 React 模板](https://vite.new/react-ts)作为开发者的起点，而不是[创建 React App](https://create-react-app.dev/) 。

高调的 React 维护者 Dan Abramov 很快回答道:

这不是尤雨溪第一次在 Twitter 上解决 React 及其生态系统的缺点。

此前，他指出了新文档网站的糟糕表现。

您还展示了 Vue.js 如何处理复杂的工作。React 中的代码需要特殊的优化，而 Vue 不需要任何额外的爱或与众不同。

**底线**

React vs Vue 的答案是……很复杂。在某些情况下，Vue 更好，在某些情况下，React 更好。然而，让人们意识到尤雨溪的解决方案更好的方法是不要对你的竞争对手发表评论。最好保持冷静，因为这样的评论通常会让你看起来没有安全感。

当谈到 Vite vs Create React App 与 Webpack 的合作时， [Vite 显然是赢家。它速度更快，也更受开发人员的欢迎。](https://www.itmagination.com/blog/360deg-it-check-35-state-of-js-rust-express-next-line-hp-firefox-chrome)React 团队面临的问题是:想象一下可口可乐将百事可乐的展台放在他们的总部内。

‍

*最初发表于*[T5【https://www.itmagination.com】](https://www.itmagination.com/blog/360deg-it-check-49-open-banking-amazon-microsoft-github-react-vue)*。*