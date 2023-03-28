# React 18 中 useEffect 钩子的巧妙行为

> 原文：<https://medium.com/geekculture/the-tricky-behavior-of-useeffect-hook-in-react-18-282ef4fb570a?source=collection_archive---------0----------------------->

![](img/75672c8dc50023f94eed682cf7f4e54e.png)

[**React 18**](/p/610d0de81336) 引入了新的发展——只查不查的严格模式。每当组件第一次装载时，这种新的检查将自动卸载和重新装载每个组件，在第二次装载时恢复以前的状态。

react 团队正在为未来将添加到 React 的**新功能铺平道路，该功能允许 React 在保留状态的同时添加和删除 UI 的部分。因为它要求组件对多次安装和破坏的影响具有弹性。您可以阅读 React 18 中新的严格模式行为的更多信息，以及 React 团队添加它们的原因:**

[](/geekculture/the-foundational-update-to-core-rendering-model-react-18-610d0de81336) [## 核心渲染模型 React 18 的基础更新

### 简介:

medium.com](/geekculture/the-foundational-update-to-core-rendering-model-react-18-610d0de81336) 

> 请通过媒体**跟随我**，获取关于下一篇新文章的通知。我也活跃在 Twitter 上 [**@IbraKirill**](https://twitter.com/IbraKirill) 。

[**本例中的 useEffect 回调**](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f) **为初始渲染运行两次。状态更改后，组件呈现两次，但效果应该运行一次。**

示例:

```
useEffect(() => {  
console.log("You will see this log twice for dev mode, once after state change - double effect call.")
}, []);
```

输出:

```
You will see this log twice for dev mode, once after state change - double effect call.You will see this log twice for dev mode, once after state change - double effect call.
```

[随着 React 18 中的严格模式](/p/610d0de81336)，React 18 中增加了`<React.StrictMode />`中的特效火两次，将模拟开发模式下的组件卸载和重装，**所以每个组件都是挂载，然后卸载，最后再重装:**

在 React 18 之前，React 将安装组件并创建效果:

```
* React mounts the component.
    * Layout effects are created.
    * Effect effects are created.
```

使用 React 18 中的严格模式，React 将模拟在开发模式下卸载和重新安装组件:

```
* React mounts the component.
    * Layout effects are created.
    * Effect effects are created.
* React simulates unmounting the component.
    * Layout effects are destroyed.
    * Effects are destroyed.
* React simulates mounting the component with the previous state.
    * Layout effect setup code runs
    * Effect setup code runs
```

我可以想象人们会对这种奇怪的行为感到惊讶，尤其是在这里的[使用效果](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f)文档中没有提到/提及的:https://reactjs.org/docs/hooks-effect.html

此外，我在变更日志中没有看到提到这个突破性的变化:[https://github . com/Facebook/react/blob/main/changelog . MD # 1800-March-29-2022](https://github.com/facebook/react/blob/main/CHANGELOG.md#1800-march-29-2022)

在[基础更新 React 18](/p/610d0de81336) 的[升级帖](https://reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html#updates-to-strict-mode)T33 中有描述。

最糟糕的问题是，当我们从 useEffect 内部的 API 获取数据时，我们会有关于在哪里放置`fetch`调用的问题，这样它们就不会在开发模式下被触发两次`[useEffect](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f)` [回调](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f)？

**我在**[**Github 关于 React 18**](https://github.com/facebook/react/issues/24502#) **来自** [丹·盖亚龙](https://github.com/gaearon) **:**

有一个建议是根本不要从`useEffect`那里拿来。有许多理由不这样做(渲染时提取导致瀑布，您开始提取太晚，效率低下，您没有一个好的地方来缓存组件之间的结果，请求之间没有重复数据删除，等等)。所以我会推荐 React Query 或类似的库。

如果您从效果中获取，您可以这样做:

```
useEffect(() => {
  let ignore = false;
  fetchStuff().then(res => {
    if (!ignore) setResult(res)
  })
  return () => { ignore = true }
}, [])
```

这不会阻止双重提取，但会忽略第一次提取的结果。所以就像从没发生过一样。开发中额外的 fetch 调用没有任何害处。您还可以使用带有取消功能的获取助手，取消获取而不是忽略其结果。请记住，在生产中，您只能获取一次。

这个*正是*正在做的事情，不是吗？如果你不想要它——不要用严格模式包装你的应用程序，但是我认为`StrictMode`做的不止这些。而在 React 17 中并没有这样做。我没有争论——我只是好奇为什么这种特殊的破坏行为不能作为选择加入。我认为这背后有很好的原因。

**Github 问题中的一个搞笑评论说关于 React 18 from @**[**cytrowski**](https://github.com/cytrowski)**:**

有来自类别的招聘编码任务:“让我看看你可以从 REST API 获取一些数据”。我可以想象招聘人员和候选人都很惊讶为什么 fetch 会被调用两次(假设他们现在在 React 17 项目中工作，并且只是为了实时编码任务才使用 CRA)。

我想在我看来，我会继续使用 React 17 一段时间，直到 React 团队解决这个问题。

> **我希望我增加价值，**如果你喜欢读这篇文章，并想支持我成为一名作家，你可以 [**请我喝杯咖啡！**](http://buymeacoffee.com/kirillibrahim)
> 
> 如果你想潜进[**用实际例子反应 18**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1411694&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-react-fullstack-course%2F) ，我用下面的 [**课程**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1411694&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-react-fullstack-course%2F) 劝你。
> 
> *如果你想一头扎进* [***最佳实践模式中反应过来***](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.2815357&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fadvanced-react-render-performance-best-practices-patterns%2F) *。我劝你用下面的* [***课程***](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.2815357&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fadvanced-react-render-performance-best-practices-patterns%2F) ***。***
> 
> 如果你想一头扎进[**服务器端渲染跟 React**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1383496&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fserver-side-rendering-with-react-and-redux%2F) ，我建议你跟下面的 [**课程**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1383496&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fserver-side-rendering-with-react-and-redux%2F) 。