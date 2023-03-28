# 反应 18 阿尔法:快速概述

> 原文：<https://medium.com/geekculture/react-18-alpha-a-quick-overview-49217cd089a5?source=collection_archive---------8----------------------->

React 18 Alpha 功能的快速概述

![](img/a6d226749e6656757a5c396591e3957c.png)

Photo by [Andreas Sjövall](https://unsplash.com/@andreassjovall?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/new-features?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

React 18 Alpha 已于上周发布，带有很酷的新功能和一个[工作组](https://github.com/reactwg/react-18)来了解社区对新功能的逐步采用。

让我们看看有哪些新功能

1.  自动配料
2.  开始过渡
3.  新悬念 SSR
4.  并行暂记

# 自动配料

我们先来看看什么是批处理？

**批处理是将多个状态更新组合到一个渲染中以优化性能。**

让我们用一个代码示例来看看这个

React 17 with Batching

尝试在 [codesandbox](https://codesandbox.io/s/react-17-batching-ehwby?file=/src/index.js) 中的演示

我们可以看到， **handleClick** 内部有两个 setState。但是当我们点击**下一个**按钮时，组件只渲染一次。您可以在控制台中看到日志。

批处理在 React 17 中已经存在，但是 React 过去只对浏览器事件进行批处理更新，而不是回调。查看下面的代码了解更多细节

React 17 without batching

尝试 [codesandbox](https://codesandbox.io/s/react-17-without-batching-fry7t?file=/src/index.js:0-943) 中的演示

我们可以看到，当点击**下一个**按钮时，组件在控制台中渲染了两次。React 不会在 promise、setTimeout 或任何其他事件中批量更新。为了克服这个问题，React 18 中增加了自动配料。

**自动批处理**在所有事件中执行批处理更新。因此，与旧版本的 React 相比，我们自动获得了更好的性能。

那么，为了让我们的应用获得更好的性能，我们需要做些什么呢？

只需将你的 react 版本升级到 18(目前是 18 Alpha)并将 **createRoot** 添加到你的 **ReactDom** 渲染中，如下所示

```
ReactDOM.createRoot(rootElement).render(<App />);
```

现在你所有的更新都会自动批量处理。查看下面的代码示例

React 18 with createRoot

尝试 [codesandbox](https://codesandbox.io/s/react-18-with-createroot-euobe?file=/src/index.js:0-1065) 中的演示

在上面的例子中，我们可以看到组件在控制台中只更新了一次，尽管状态是在承诺中更新的。它很酷，它将通过避免**不需要的渲染**来提高应用程序的性能。

如果我们不想批量更新，我们可以使用下面的 **flushSync**

```
import { flushSync } from 'react-dom'; // Note: react-dom, not reactfunction handleClick() {
  flushSync(() => {
    setCounter(c => c + 1);
  });
  // React has updated the DOM by now
  flushSync(() => {
    setFlag(f => !f);
  });
  // React has updated the DOM by now
}
```

# 开始过渡

开始转换将状态更新分为两种类型

1.  紧急更新
2.  过渡更新(慢速更新)

开始过渡主要关注 app 的**用户体验**。因为 transition 内部的更新在后台运行缓慢。

检查下面的代码

```
import { startTransition } from 'react';// Urgent: Show what was typed
setInputValue(input);// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setSearchQuery(input);
});
```

**如果有类似用户交互事件的紧急更新，setSearchQuery** 将被中断。

反应提供一个**挂钩**用于与**悬挂**的过渡

```
import { useTransition } from 'react';const [isPending, startTransition] = useTransition();
```

**isPending** 可用于向用户显示装载状态。如果过渡正在进行中。

React 建议对 UI 中的远程数据和大数据更新使用**转换**。

# 新悬念 SSR

该特性用于在服务器中呈现 react 组件。现在服务器端渲染也支持悬念。

首先让我们看看什么是 **SSR** ？

**SSR** 从服务器上的 React 组件生成 HTML，并将该 HTML 发送到客户端。SSR 让用户在 JavaScript 包加载和运行之前看到页面的内容。

## SSR 的缺点

1.  整个 HTML 需要在服务器中呈现并下载，以便向用户显示 UI。
2.  需要等到所有的 JS 都下载完，才能让组件**交互**。

这使得 UX 对于用户来说是非常糟糕的体验。为了克服这个问题，React 引入了两个新特性

SSR 的两个主要特征是

1.  流式 HTML
2.  选择性水合

# 流式 HTML

有了流 HTML，react 将发送静态 HTML，如标题，菜单，只要它们准备好了就发送给客户端，并在它准备好流时加载沉重的组件(这取决于数据库中的数据，如评论组件)。所以现在用户不需要等待，就可以看到初始的 UI 渲染。

但是，渲染的 UI 不是交互式的，它需要等到 JS 被加载。所以这里**选择性水合**开始发挥作用

> **水合是将 JS 连接到服务器生成的 HTML 的过程。**

# **选择性水合**

选择性水合优先考虑哪个组分 JS 需要首先装载。当组件加载正在进行时，如果用户试图与任何组件进行交互。React 将检测该事件并首先水合相互作用组分。

这些新的 SSR 功能将解决

1.  不再等待在服务器上渲染所有的 HTML
2.  不再等待加载所有的 JS 来使组件交互
3.  不再等待所有组分水合以与 a 组分相互作用。

# 并行暂记

现在悬疑来了全力支持。喜欢

1.  延迟转换(在继续状态转换之前等待数据解析)。
2.  占位符限制(通过限制嵌套的连续占位符的外观来减少 UI 抖动)。
3.  SuspenseList(协调组件列表或网格的外观，比如按顺序排列它们)

检查悬念示例

```
<Suspense fallback={<Loading />}>
  <ComponentThatSuspends />
  <Sibling />
</Suspense>
```

在上面的例子中，React 将首先显示`<Loading />`组件，然后在`<ComponentThatSuspends/>.`中解析数据时将`<Loading />`组件替换为`<ComponentThatSuspends />`和`<Sibling/>`

**React 18 Concurrent suspension**的新变化是`<Suspense />`组件内的任何东西都不会被渲染，直到数据被解析！

但是在**遗留悬念(React 17 中的悬念)**兄弟组件被立即挂载到 DOM，它的效果和生命周期被触发，并且对 UI 隐藏。

使用 React 核心团队分享的示例，检查**遗留暂挂**和**并发暂挂**之间的差异。

遗留悬念示例—[https://codesandbox.io/s/keen-banach-nzut8?file=/src/App.js](https://codesandbox.io/s/keen-banach-nzut8?file=/src/App.js)

并发暂挂实例—[https://codesandbox.io/s/romantic-architecture-ht3qi?file=/src/App.js](https://codesandbox.io/s/romantic-architecture-ht3qi?file=/src/App.js)

# **现在让我们试试 React Alpha**

要安装最新的 React 18 alpha，请使用`@alpha`标签

```
npm install react@alpha react-dom@alpha
```

从 Alpha 版到 Beta 版需要几个月的时间，到稳定版需要更多的时间。查看 React [**工作组**](https://github.com/reactwg/react-18) 了解更多详情。

# 资源

*   react Blog—[https://react js . org/Blog/2021/06/08/the-plan-for-react-18 . html](https://reactjs.org/blog/2021/06/08/the-plan-for-react-18.html)
*   React 18 工作组—[https://github.com/reactwg/react-18](https://github.com/reactwg/react-18)