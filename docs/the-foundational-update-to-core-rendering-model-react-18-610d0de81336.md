# 核心渲染模型 React 18 的基础更新

> 原文：<https://medium.com/geekculture/the-foundational-update-to-core-rendering-model-react-18-610d0de81336?source=collection_archive---------1----------------------->

![](img/845a9d700cfc3dc5b9d92b31e5ef087c.png)

# 简介:

React 18 于 2022 年 3 月 8 日发布，有新的更新和改进。最重要的更新是并发渲染模型，它是 React 核心渲染模型的基础更新，大多数新功能都是为了利用它而构建的。React 18 只是 React 团队在这个新基础上构建的目标的开始。

正如我们一直期待 React 团队的重大改进，以及一个简单的升级指南，而没有我们旧项目的负面影响或昂贵的重写成本。

> 请通过媒体**跟随我**，获取下一篇新文章的通知。我也活跃在推特 [**@IbraKirill**](https://twitter.com/IbraKirill) 。

**我们将在本文中解释:**

1-并发机制

React 18 中的新功能。

React 18 中的新 [**挂钩**](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f) 。

4-折旧

5-React 18 升级指南

# 1-并发机制:

什么是并发机制？当我们同时做不止一件事时，它就会发生。不要与并行性混淆，并发性是指多个操作序列在重叠的时间段内运行。它们是互不依赖的操作。

> ***例 1:*** 我有一个朋友打来的电话，当我正在和他说话的时候，我的父亲打电话给我，我觉得这可能是一个紧急电话，所以我告诉我的朋友，我会让他等一下，然后接听我父亲的电话，之后再切换到我朋友的电话。
> 
> **与此同时，我的 omlet 还在煮。我会把重点放在阅读和烹饪过程中的各个点上什么是更紧急的。**

并发并不一定意味着我同时与两个人交谈或者同时做饭阅读，这不是一种并行技术。这只是意味着，在任何时刻，我可能正在进行多个电话或行动，我选择与谁交谈或我将在哪个行动上取得进展，基于哪个对话或行动更紧急。

在 React 的早期版本中，有一种方法称为分块渲染，更新在单个不间断的同步渲染中进行渲染，因此一次只能处理一个任务。使用同步呈现，一旦更新开始呈现，没有什么可以中断它，直到用户可以在屏幕上看到结果。

**在 React 18 中:**引入了并发特性，使得渲染可中断。应用程序可能会开始呈现更新，中间暂停，然后稍后继续。React 保证 UI 看起来是一致的，即使渲染被中断。要做到这一点，它会等到完成整个树的评估后，再执行 DOM 突变。有了这个功能，React 可以在后台准备新的屏幕，而不会阻塞主线程。这意味着即使在大型渲染任务中，用户界面也可以立即响应用户输入，创造流畅的用户体验。

React 中的并发模式不是一个特性，但它是 React 核心渲染模型的一个基础更新。它是一种新的幕后机制，使 React 能够同时准备多个版本的 UI。您可以将并发性视为一个实现细节。它的价值在于它所开启的功能。React 在其内部实现中使用了复杂的技术，比如优先级队列和多重缓冲。但是你不会在我们的公共 API 中看到这些概念。因此，虽然知道并发性是如何工作的并不是非常重要，但在高层次上了解它是什么可能是值得的。

因此，为了在 React 上应用并发性，我将@sylwiavargas 的一些解释放到了 Github 上:

> 现在，为了解释这个类比，在 React 的情况下,“电话”就是你的`*setState*`电话。以前，React 一次只能处理一个状态更新。所以所有的更新都是“紧急的”:一旦你开始重新渲染，你就不能停下来。但是使用`[*startTransition*](https://github.com/reactwg/react-18/discussions/41)`，你可以将一个非紧急更新标记为过渡。
> 你可以把“紧急”`*setState*`更新理解为类似于紧急电话(例如，你父亲需要你的帮助)，而转换就像轻松的对话，如果不再相关，可以暂停甚至中断。

React 团队正计划在 React 18 即将推出的次要版本中添加一个名为`<Offscreen>`的新组件，你将能够使用屏幕外在后台准备一个新的 UI，以便在用户展示它之前准备好，例如:当用户从一个屏幕切换回来时，React 应该能够将上一个屏幕恢复到它之前的状态。

> 正如安德鲁·克拉克在 React 18 主题演讲 : **中所说:“有了 React，设计师和开发人员就有了共同的语言。”**

# React 18 中的新功能:

## a-自动配料:

在 React 18 之前，当我们在 promises、setTimeout、本机事件处理程序或任何其他事件中有多个状态更新组时，例如:

它将为三个状态更新执行 3 次重新渲染，当然，这对性能不利。

React 18 引入了自动批处理，React 将 promises、setTimeout、native 事件处理程序或任何其他事件中的多个状态更新分组到单个重新渲染中以获得更好的性能，因此在升级到 React 18 后，上述示例将只重新渲染一次。

## B-悬念功能:

在 React 18 之前，当我调用 API 来获取数据时，加载状态会以微调器或任何类型的 UI 加载组件的形式出现在用户面前，当我使用完 API 时，我们会消失微调器，另一个包含数据的组件会出现。

悬念使“UI 加载状态”成为 React 编程模型中的一级声明性概念。如果组件树的一部分还没有准备好被显示，悬念可以让你声明性地指定它的加载状态。它减少了代码行数，代码变成了干净的代码:

React 18 中的悬念与过渡 API 结合使用效果最佳。如果您在过渡期间暂停，React 将防止已经可见的内容被回退替换。相反，React 将延迟渲染，直到加载了足够的数据，以防止出现错误的加载状态。

**服务器暂停:**

[**客户端渲染:**](https://kirillibrahim.medium.com/gray-area-on-when-to-use-different-rendering-modes-csr-ssr-ssg-214a636a24a4) 服务器渲染一个空白页面，脚本标签指向 app 的捆绑包。空白页面被发送到客户端浏览器，浏览器开始运行应用程序，编译所有内容，然后进行 API 调用并呈现页面内容。

如果 JavaScript 包很大，或者您的连接速度很慢，这个过程可能需要很长时间，用户将等待页面变得可交互，或者看到有意义的内容。

我们可以使用服务器渲染来改善用户体验，避免用户坐在空白屏幕上。

[**服务器端呈现:**](https://kirillibrahim.medium.com/gray-area-on-when-to-use-different-rendering-modes-csr-ssr-ssg-214a636a24a4) 是应用程序通过在服务器上显示网页而不是在浏览器中呈现网页的能力。这意味着，如果您有一个服务器端呈现的应用程序，您的内容将在服务器端获取，并传递到您的浏览器以显示给您的用户。这允许用户在 JS 包加载时和应用变得可交互之前查看一些 UI。

如果我们的应用程序中的大多数组件都很快，除了其中的一两个，所以一个慢的组件可以减慢整个页面，这个组件的渲染通常是应用程序中的瓶颈，增加了渲染时间。

在 React 18 之前，我们不能通知 React 推迟加载一个慢组件，也不能通知 React 发送其他组件的 HTML，直到慢组件加载完成。

React 18 在服务器上增加了对悬念的支持。我们可以将应用程序的慢速部分封装在悬念组件中，告诉 React 延迟慢速组件的加载&指定一个加载状态，当它像加载微调器一样加载时可以显示，重点是向下发送另一个，当慢速组件准备好并获取其数据时，服务器渲染器将在同一流中弹出其 HTML。

因此，用户可以尽快看到页面的框架，并随着 HTML 的到来，看到它逐渐显示更多的内容。在页面上加载任何 JS 或 React 之前，所有这些都已完成，从而改善了用户体验并降低了感知延迟。

> 如果你想一头扎进[**服务器端渲染用 React**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1383496&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fserver-side-rendering-with-react-and-redux%2F) ，我劝你用下面的 [**课程**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1383496&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fserver-side-rendering-with-react-and-redux%2F) 。

## c 转换:

并发是指多个操作序列在重叠的时间段内运行。Transition 是 React 引入的，它将帮助我们利用并发机制来区分紧急和非紧急更新。

**紧急更新**需要即时响应，以匹配用户对物理对象如何操作(如打字、点击、按压等)的直觉和经验。例如，如果当用户点击产品的过滤器按钮时有轻微的延迟，这对用户来说是糟糕的体验，因为用户期望立即的响应，但是如果用户注意到在过滤后显示产品的结果有轻微的延迟，这种小的延迟将是察觉不到的，并且通常是预期的。

向用户显示被点击按钮的反射是重要的(即时响应),因此是紧急的。显示产品过滤后的结果不是那么紧急，所以可以标记为非紧急。

**这些非紧急更新被称为转换。**过渡是不同的，因为用户不期望像紧急更新那样在屏幕上看到每个中间值。通过将非紧急 UI 更新标记为“过渡”，React 将知道哪些更新要优先处理。通过这种方式，摆脱陈旧渲染&优化渲染变得更加容易。

您可以使用`startTransition API`将更新标记为非紧急。`startTransition`通知 React 哪些更新是紧急的，哪些是“过渡”。这里有一个例子:

包装在`startTransition`中的更新被视为非紧急更新，如果出现更紧急的更新，如点击或按键，更新将被中断。如果过渡被用户打断(例如，在一行中键入多个字符)， **React 将丢弃未完成的陈旧渲染工作，只渲染最新的更新，因此，如果在结果渲染完成之前再次更改过滤器，您只需查看最新的结果。**

## d-新的严格模式行为:

React 18 引入了一个巨大的突破性变化，在严格模式下。所有组件都被装载和卸载，然后再次装载。

在反应 18:

React 18 之后，React 将在开发模式下模拟卸载和重新安装组件:

react 团队正在为 [**将来会添加到**](https://github.com/reactwg/react-18/discussions/19)React 的新功能铺平道路，比如“屏幕外”API，允许 React 通过隐藏组件而不是卸载组件来保留这样的状态。例如，当用户从一个屏幕切换回来时，React 应该能够立即显示上一个屏幕。为此，React 将使用与以前相同的组件状态卸载和重新装载树。

**在 React 18 中的严格模式下，useEffect 钩子有棘手的行为:**

[](https://kirillibrahim.medium.com/the-tricky-behavior-of-useeffect-hook-in-react-18-282ef4fb570a) [## React 18 中 useEffect 钩子的巧妙行为

### React 18 引入了一个新的开发专用的严格模式检查。这个新检查将自动卸载和重新装载…

kirillibrahim.medium.com](https://kirillibrahim.medium.com/the-tricky-behavior-of-useeffect-hook-in-react-18-282ef4fb570a) 

# React 18 中的新挂钩:

*   **useId**
*   **使用过渡**
*   **使用插入效果**
*   **已用导出值**
*   **useSyncExternalStore**

我会写一篇新文章解释这些新的 [**钩子**](/swlh/react-hooks-from-scratch-a-z-bf8f7b404f7f) 。

# 4-折旧:

*   `react-dom` : `ReactDOM.render`已被弃用。如果我们使用它，我们的应用程序会发出警告，并在 React 17 模式下运行。
*   `react-dom` : `ReactDOM.hydrate`已被弃用。如果我们使用它，我们的应用程序会发出警告，并在 React 17 模式下运行。
*   `react-dom` : `ReactDOM.unmountComponentAtNode`已弃用。
*   `react-dom` : `ReactDOM.renderSubtreeIntoContainer`已弃用。
*   `react-dom/server` : `ReactDOMServer.renderToNodeStream`已被弃用。

# 5-React 18 升级指南:

**好消息:**我们不会改变我们的整个代码，或者像 Angularjs 1 到 Angular 2+所发生的那样重新学习一切，我们仍然按照以前的方式编写 React 代码。

## a-从 npm 或 yarn 安装 React 18 和 React DOM:

React 18 引入了新的根 API，为管理根提供了更好的人机工程学。在`index.js`中，我们应该将`ReactDom.render`替换为`ReactDom.createRoot`来创建一个根，并使用根来渲染你的应用。

React 团队也将`unmountComponentAtNode`更改为`root.unmount`:

如果 React app 使用服务器端渲染和水合，将`hydrate`升级到`hydrateRoot`:

## b-如果 React 项目使用 TypeScript:

我们需要将`@types/react`和`@types/react-dom`依赖项更新到最新版本。新的类型更加安全，并且捕捉到了过去被类型检查器忽略的问题。最显著的变化是`children`道具现在需要在定义道具时明确列出；

## c-采用并发功能:

我们可以在开发过程中使用`[<StrictMode>](https://reactjs.org/docs/strict-mode.html)`来帮助解决与并发相关的错误。严格模式不会影响生产行为，但是在开发过程中，它会记录额外的警告，并双重调用被认为是幂等的函数。它不会捕捉到所有内容，但它可以有效地防止最常见类型的错误。

# 结论:

本文涵盖了 React 18 推出的大部分新东西。React 团队正在为在 React 的下一个版本中添加许多新功能铺平道路。

> **我希望我增加了价值，**如果你喜欢读这篇文章，并想支持我成为一名作家，你可以 [**请我喝杯咖啡！**](http://buymeacoffee.com/kirillibrahim)
> 
> 如果你想潜进[**反应 18**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1411694&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-react-fullstack-course%2F) 用实际例子，我奉劝你用下面的 [**课程**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.1411694&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-react-fullstack-course%2F) 。
> 
> 如果您想深入 React 中的 [**最佳实践模式。我用下面的**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.2815357&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fadvanced-react-render-performance-best-practices-patterns%2F) **[**课程**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=1060092.2815357&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fadvanced-react-render-performance-best-practices-patterns%2F) **劝你。****