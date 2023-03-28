# 用 React 中的获取 API 从 API 获取数据

> 原文：<https://medium.com/geekculture/fetching-data-from-an-api-with-the-fetch-api-in-react-5dbe0abcfb41?source=collection_archive---------8----------------------->

![](img/e3d5cfd806fb087720dccb8995de264b.png)

Photo by [Eneida Nieves](https://www.pexels.com/@fariphotography?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/brown-cabin-in-the-woods-on-daytime-803975/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

所以您想用 JavaScript 从某个地方获取数据，但不确定如何去做？好吧，不要再看了！今天，我想向您展示如何使用 Fetch API，它现在是内置的 JavaScript。我们将在本指南中介绍一些内容。我们将设置一个类组件，设置我们的`state`，然后执行对一个活动 API 的获取，然后在实际的网站上显示我们的结果。

首先，我们可能应该回顾一下我们今天将使用的 API。我们将使用这个 API->[http://www.dnd5eapi.co/docs#intro](http://www.dnd5eapi.co/docs#intro)，因为它是免费的，我们不需要注册一个密钥。如果你去看文档，有相当多的数据我们可以访问。我们将只使用`/api/classes`，它将为我们提供一些数据，我们可以将这些数据呈现到页面上的列表中。

正如你在下面看到的，我们正在使用一个基于类的组件，它在结构上不同于一个函数组件。如果你来自面向对象的背景，你可能会有宾至如归的感觉。对于我们其余的人，我会解释这里发生了什么。

你首先会注意到的是`constructor`。它可用于将事件处理程序绑定到组件和/或初始化组件的本地状态。在我们的例子中，我们正在设置初始状态，这将是在页面加载时设置的。

接下来，我们使用一个 Reacts 生命周期挂钩`componentDidMount()`。`componentDidMount()`在组件挂载(插入到树中)后立即被调用。在这里，我们叫取货，并履行承诺。我们在这里根据是否得到`result`或`error`来更新我们的状态，稍后我们可以在`render()`中使用它们。

最后，我们调用`render()`，并显示相关信息。当我写逻辑时，我喜欢把它分成小函数，我可以在相关的地方传入，而不是有大块的 JSX。我发现将逻辑提取到小函数中更容易维护，对于未来的开发人员来说也更容易阅读。

这方面的代码如下:

如果你想了解更多关于获取 API 的内容，可以在这里找到。有其他选择，主要是 Axios。Axios 的官方文档是这里的。如果你想了解更多，这里有一篇很好的文章，比较了它们各自的优缺点，可以在这里找到。

我希望这篇文章是令人愉快的。如果你有任何反馈，发表评论，让我知道我可以改进的地方。如果你想看看我的其他帖子，可以在这里找到。我写的都是前端特有的东西，所以我在 [React](https://blog.devgenius.io/how-do-i-function-react-function-components-in-a-nutshell-59f2521f6d06) ， [TypeScript](https://jgrice01.medium.com/typescript-understanding-the-basics-a2264759cd2d) ，[SASS](/codex/writing-better-sass-with-dynamic-class-generators-e486a0413d0d)&[electronic](https://jgrice01.medium.com/want-to-build-desktop-apps-using-js-say-hello-to-electron-4f862c3b4e38)上有帖子。感谢您的阅读，祝您愉快！