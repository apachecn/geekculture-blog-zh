# 啦啦土地建设我们自己的反应定制钩

> 原文：<https://medium.com/geekculture/la-la-land-of-building-my-own-react-custom-hook-75f0a0fe53f9?source=collection_archive---------29----------------------->

![](img/ca25b69a0a3266cb9908ab4b13969a18.png)

# 简介:

自从 React 16.8 增加了钩子之后，很多开发者爱上了 React。他们开始用钩子来制作他们的应用程序，钩子赋予他们超越基于类的组件(如 sugar 语法)的能力，从包装器地狱中逃脱，没有任何重复地重用有状态逻辑，从难以理解、测试或维护的复杂组件中逃脱。

如果你不知道 React 钩子，你可以看看下面的文章:

[](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f) [## 从头开始反应钩子 A-Z

### 挂钩简介:

medium.com](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f) [](/swlh/react-redux-hooks-5e5dbb52d057) [## React Redux 中钩子 API 的魔力

### 简介:

medium.com](/swlh/react-redux-hooks-5e5dbb52d057) 

在本文中，我们将讨论三个要点:

1-什么是自定义挂钩？

2-如何建立和使用一个定制的钩子？

3-自定义挂钩的实际示例。

> ***请跟随我*** *过媒* ***，获取下一篇新文章的通知。*** *我也活跃在 Twitter 上*[***@ IbraKirill***](https://twitter.com/IbraKirill)*。*

# 什么是自定义挂钩？

传统上，在 React 中，我们有两种流行的方式在组件之间共享有状态逻辑:[渲染道具](https://reactjs.org/docs/render-props.html)和[更高级的组件](https://reactjs.org/docs/higher-order-components.html)。由于 React 16.8 增加了钩子，我们可以构建自己的钩子，这种方式可以让你将组件逻辑提取到可重用的函数中。

让我们假设我们有两个组件，`personProfile`和`FriendListItem,` 在这两个组件中，我们需要通过 id 来检查特定人的状态，如果一个人在线或不在线，那么我将在这两个组件中的 useEffect 中编写的内容与 chatAPI 通信，使用相同的逻辑，在这种情况下，我们希望在一个地方有重复的逻辑自定义钩子。

作为一名 React 开发人员，学习创建定制挂钩来解决问题或在自己的 React 项目中添加缺失特性的过程非常重要。

自定义钩子是一个 JavaScript 函数，它的名字以“use”开头，可以调用其他钩子。

# 如何构建和使用自定义挂钩:

自定义钩子是一个以单词“use”开头的函数，可能会调用其他钩子。“useWhatever”命名约定主要是为了让 linter 在使用这些钩子时发现错误。如果没有它，我们就无法判断某个函数内部是否包含对钩子的调用，在这种情况下，函数的使用违背了钩子的规则。

例如，下面的 useFriendStatus 是我们的第一个自定义钩子(下面的例子来自官方文档):

记住组件和钩子都是函数，所以我们在这里并没有创造任何新概念。我们只是将代码重构为另一个函数，使其可重用。

这里面没有什么新东西——逻辑从两个组件`personProfile`和`FriendListItem` 转移到一个共享的可重用钩子，确保只在自定义钩子的顶层无条件调用其他钩子，并遵循所有 React 钩子规则:只在顶层调用钩子，应该遵循顺序，并且只从 React 函数调用钩子&自定义钩子。

与 React 组件不同，自定义挂钩不需要有特定的签名。我们可以决定它接受什么作为参数，以及它应该返回什么(如果有的话)。换句话说，它就像一个普通的函数。它的名字应该总是以`use`开头，这样你一眼就能看出挂钩 **的 [**规则适用于它。**](https://reactjs.org/docs/hooks-rules.html)**

现在我们已经将这个逻辑提取到一个地方一个`useFriendStatus`钩子，我们可以*在其他两个组件中使用它:*

它以完全相同的方式工作。如果您仔细观察，您会注意到我们没有对行为进行任何更改。我们所做的就是将两个函数之间的一些公共代码提取到一个单独的函数中。

**定制钩子是钩子设计的自然结果，而不是 React 特性。**

> *如果你想一头扎进**[***React 18***](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1411694&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-react-fullstack-course%2F)**用实际例子，我奉劝你用下面的* [***课程***](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1411694&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-react-fullstack-course%2F) *。***

## **两个重要注意事项:**

**使用相同钩子的两个组件共享状态吗？不会，自定义钩子是一种重用*有状态逻辑*的机制(比如设置订阅，记住当前值)，但是每次使用自定义钩子，里面的所有状态和效果都是完全隔离的。**

****自定义钩子如何获得隔离状态？**每一个*调用*到一个挂钩就得到一个隔离状态。因为我们直接调用 useFriendStatus，所以从 React 的角度来看，我们的组件只调用 useState 和 useEffect。而且就像我们[前面学过](https://reactjs.org/docs/hooks-state.html#tip-using-multiple-state-variables)一样，我们可以在一个组件中多次调用 useState 和 useEffect，它们会完全独立。**

## ****钩子之间传递信息:****

**钩子是函数，我们可以在它们之间传递信息。为了说明这个概念，我们将从官方文件中举例说明以下例子来解释:**

**这是一个聊天消息收件人选择器，显示当前选定的朋友是否在线:**

**我们将当前选择的朋友 ID 保存在 recipientID 状态变量中，如果用户在<select>选择器中选择了不同的朋友，就更新它。</select>**

**如果您注意到我们在以下钩子之间传递信息:**

**useState 挂钩调用为我们提供了 recipientID 状态变量的最新值，我们可以将它作为参数传递给我们的自定义 useFriendStatus 挂钩:**

```
**const [recipientID, setRecipientID] = useState(1);const isRecipientOnline = useFriendStatus(recipientID);**
```

**这让我们知道当前选择的*朋友是否在线。如果我们选择一个不同的朋友并更新 recipientID 状态变量，我们的 useFriendStatus 钩子将取消订阅之前选择的朋友，并订阅新选择的朋友的状态。***

**我建议欧洲大学 [**在线学位**](https://click.linksynergy.com/fs-bin/click?id=GGg4no0HUcA&offerid=871625.130&subid=0&type=4) 课程的读者，其中许多是免费的 [**。**](https://click.linksynergy.com/fs-bin/click?id=GGg4no0HUcA&offerid=871625.130&subid=0&type=4)**

# **构建定制钩子的实际例子:**

## **使用 Fetch:**

**获取数据是许多前端开发人员在构建任何 react 应用程序时经常做的事情，我们获取应用程序中许多组件的数据。**

**无论您选择何种方式获取数据，无论是使用 [Axios](https://github.com/axios/axios) ，还是 [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) ，或者其他任何方式，您总是在 React 组件中一遍又一遍地编写相同的代码。**

**我们将构建一个包含重复代码的自定义钩子，当我们需要在应用程序中获取数据时，我们可以调用它。**

**`useFetch`钩子将有参数，我们需要查询的 URL&获取数据，以及一个表示我们希望应用于请求的选项的对象，这里我们将使用获取 API 方式来发出请求，因此一旦承诺被解析，我们就通过解析响应体来检索数据。为此，我们使用`json()`方法，我们还应该捕捉并处理网络错误，以防我们的请求出现问题&还向我们的用户指示异步请求的状态，例如在呈现结果之前显示一个加载指示器(一个加载微调器),同时请求正在运行，以便我们的用户知道我们正在获取数据。**

**我们的`useFetch`钩子将返回一个对象，该对象包含从 URL 获取的数据或者错误，如果发生了任何错误的话&异步请求的状态。**

```
**return { loading error, data };**
```

****在我们的组件中使用 use fetch:****

> **如果你喜欢看文章，想支持我当作家，你可以 [**请我喝咖啡！**](http://buymeacoffee.com/kirillibrahim)**
> 
> **如果你想深入研究 [**高级 React 组件模式**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.2690172&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-guide-to-advanced-react-patterns%2F) &理解为什么设计模式很重要&通过模仿真实世界的例子来学习。我用下面的 [**课程来劝你。**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.2690172&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-guide-to-advanced-react-patterns%2F)**
> 
> **如果你想深入 JavaScript 中的 [**设计模式**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.2251868&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fdesign-patterns-javascript%2F) &探索 JavaScript 中设计模式的现代实现。我建议你用下面的[T22 课程。](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.2251868&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fdesign-patterns-javascript%2F)**