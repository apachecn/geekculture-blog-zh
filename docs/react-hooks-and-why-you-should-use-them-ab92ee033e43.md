# React 挂钩以及为什么应该使用它们

> 原文：<https://medium.com/geekculture/react-hooks-and-why-you-should-use-them-ab92ee033e43?source=collection_archive---------16----------------------->

什么是钩子，我为什么要用它？React 文档将钩子定义为*一个特殊的函数，可以让你“挂钩”React 特性。*钩子是完全可选的，不会取代任何 React 概念，相反，它们为 React 概念提供了一个更直接的 API，如 props、state、context、refs 和 lifecycle。钩子不仅更容易使用和测试，而且对于开发人员编写和阅读来说，它们也使代码更干净、更清晰。

要学习的两个大钩子是状态钩子和效果钩子。状态挂钩允许您向功能组件添加本地状态。效果挂钩增加了向功能组件添加“副作用”的能力；副作用与类生命周期组件的目的相同，但是在单个 API 中。类组件可能会因代码过多而变得不堪重负，而且对于它们的用途来说似乎太长了。

```
import React, { Component } from 'react';class Counter *extends* Component {constructor(props) {super(props);this.state = {count: 0}this.handleClick = this.handleClick.bind(this)}handleClick(e){e.preventDefault();this.setState({count: this.state.count + 1})}render() {return (<div><p>The count is: {this.state.count}</p><button *onClick*={this.handleClick}>Click me</button></div>)}}*export* default Counter;
```

通过使用钩子，这个类组件现在可以用更少的代码行来编写，这样更容易理解，也更简洁。上面的代码可以用更少的代码行编写，用下面的钩子写得更紧凑。

```
import React, { useState } from 'react';function Counter() {const [count, setCount] = useState(0)return (<div><p>The count is: {count} </p><button *onClick*={() => setCount(count + 1)}>Click me</button></div>)}*export* default Counter;
```

使用钩子使得代码和有状态逻辑可重用，一个巨大的好处是它们不会像高阶组件一样在 DOM 中创建另一个元素，从而摆脱了 hoc 可能带来的“包装器地狱”。开发人员的目标是通过在编译时消除死代码来提高性能，让钩子在未来的 React 优化中更好地工作。JavaScript 需要编译的代码越少，意味着运行时需要执行的代码就越少。这就是为什么效果钩子可能是你应该学习的一个钩子。

Effect hook 不仅结合了重复的代码并提高了性能，它还帮助开发人员避免了生命周期方法的常见错误。钩子帮助开发者避免不恰当地使用*组件更新*生命周期方法。默认情况下，UseEffect 在第一次渲染后和每次更新后都运行。它总是在应用下一个效果之前清除之前的效果。

尽管“清理”或在每次渲染后应用效果可能会导致性能问题。useEffect 挂钩允许您传递第二个参数来比较从上一个渲染传递到下一个渲染的内容。如果在下一次渲染和上一次渲染之间传递到参数中的项目是相同的，React 将跳过效果，从而节省一些时间并优化代码。

自定义钩子是减少重复代码和通过创建“第三方”函数在两个 JavaScript 函数之间共享逻辑的好方法。React 文档将自定义钩子定义为*，这是一个 JavaScript 函数，其名称以* `*use*` *开头，并且可以调用其他钩子。*以前在组件之间共享有状态逻辑的两种流行方式是渲染道具和 hoc。钩子解决了 hoc 和渲染道具的许多问题，但钩子不会强迫你向“树”添加组件，也不会强迫你进入包装器或回调地狱。

在应用程序中使用钩子时，只有几条牢不可破的规则。

*   **只调用顶层的钩子**

不要从条件、嵌套函数或循环内部调用钩子。遵循这一规则将有助于确保每次渲染都以相同的顺序调用钩子，这使得 React 能够在多个 *useState* 和 *useEffect* 调用之间保持钩子的状态。React 依赖于钩子被调用的顺序来知道哪个状态对应于哪个调用。

*   **仅从 REACT 函数调用钩子**

不要从 JavaScript 函数中调用钩子，只从 React 函数组件和自定义钩子中调用。通过将有状态逻辑放在清晰可见的源代码中，您只会使代码更易读、更易理解。

如果你发现自己难以遵循这些规则，React 发布了一个名为 eslint-plugin-react-hooks 的 ESLint 插件来强制执行这些规则。该插件默认包含在 Create-React-App 中。如果您选择了不同的 React 创建路线，您可以通过运行 NPM install eslint-plugin-React-hooks—save-dev 轻松添加插件。

钩子在这里不是用来代替 React 中的类的，它们仅仅是一种可选的方式来编写它们，并显示 React 的真实本质，它是功能性的。React 中的类很容易在代码中犯错误和产生 bug，而钩子的目的是让开发人员更容易解决这些 bug，并使代码更容易重用。当在单个组件上使用多个定制钩子时，钩子没有缺点。

# 参考

*   react . js Docs—[https://reactjs.org/docs/hooks-intro.html](https://reactjs.org/docs/hooks-intro.html)
*   React Hooks 与旧的可重用逻辑方法的优点和比较，作者 mate usz Roth-[https://medium . com/@ mate usz Roth/react-Hooks-advantage-and-comparison-to-older-reusable-logic-approach-in-short-f424c 9899 cb5](/@mateuszroth/react-hooks-advantages-and-comparison-to-older-reusable-logic-approaches-in-short-f424c9899cb5)
*   React Hooks 的最佳实践作者 David Aden eye—[https://www . smashingmagazine . com/2020/04/React-Hooks-best-Practices/](https://www.smashingmagazine.com/2020/04/react-hooks-best-practices/)