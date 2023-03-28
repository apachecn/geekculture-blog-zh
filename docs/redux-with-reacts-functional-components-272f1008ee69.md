# Redux with React 的功能组件

> 原文：<https://medium.com/geekculture/redux-with-reacts-functional-components-272f1008ee69?source=collection_archive---------0----------------------->

## 如何使用 Redux 的 useSelector 和 useDispatch 钩子

![](img/88273c5b489e9dfddf6a6442cc1b9337.png)

在阅读本文之前，您应该熟悉 Redux 是什么以及如何在 React 类组件中使用它。

# 设置

首先创建一个新的 react 应用程序。我将使用一个名为 Redux Toolkit 的包来快速设置应用程序的外壳。如果您想进行同样的跑步:

```
npx create-react-app my-app --template redux
```

这将创建您的 React 应用程序，使用官方模板来使用 Redux。然后在您的应用程序中运行:

```
npm install @reduxjs/toolkit react-redux
npm install redux
```

在 src/app/store.js 中创建以下内容:

然后，转到 index.js，像这样包装您的应用程序组件:

现在你已经准备好了。

## 计数器组件

对于这个例子，我们将创建一个简单的计数器。

当使用类组件时，您会看到类似这样的内容:

这是标准的 Redux 设置，其中我们使用 mapStateToProps 来访问 Redux 存储上的属性，这些属性可作为 Props 供我们的组件使用，然后将操作分派给我们的 reducer(也可作为 props 使用),以便更新这些属性。

然而当使用钩子时，我们需要做一些不同的事情。

# 使用选择器

useSelector 钩子是我们如何在组件中模拟 mapStateToProps 函数的。

首先从“react-redux”导入 useSelector。

```
import { useSelector} from 'react-redux'
```

与 useState 挂钩非常相似，您将在组件顶部声明一个变量，并将其设置为等于 useSelector 函数的返回值。useSelector 函数将另一个函数作为其参数。这个函数将传递整个状态对象作为它的参数。

```
const count = useSelector((state) => state.counter)
```

因此，基本上，您正在编写一个以 state 为参数的函数，然后返回该 state 对象的一些属性(在本例中是计数器的值)。然后，您将该函数传递给 useSelector，后者赋予该函数访问状态对象的权限。然后，在函数内部将函数的返回值赋给声明的变量(在本例中为 count)。

对你来说够困惑了吧？

本质上是这样的:

```
const mapStateToProps = (state, props) = { counter: state.counter, user: state.user}
```

还有这个:

```
const count = useSelector((state) => state.counter)const user = useSelector((state) => state.user)
```

做着完全相同的事情，只是语法略有不同。

# 使用显示器

在我看来，useDispatch 其实简单多了。这让我想起了很多使用历史。

首先，我们需要导入它:

```
import { useSelector, **useHistory**} from 'react-redux'
```

然后，您需要做的就是将它放在组件的顶部:

```
const dispatch = useDispatch()
```

现在，您有一个名为 dispatch 的函数，它会将您传递的任何内容发送给 reducer。它实际上比 mapDispatchToProps 简单得多。就像之前我们想从 redux/actions 导入我们的动作一样

```
import { incrementCounter, decrementCounter } from '../redux/actions/counterActions.js'
```

但是，我们可以这样做，而不是完成将这些功能添加到另一个功能，然后将外部功能映射到我们的道具的整个复杂过程:

```
onClick={() => dispatch(decrementCounter())}
```

不需要任何道具，我们在 JSX 中用一行代码就定义了这个函数！

最终组件将如下所示:

注意，我们不再需要组件导出底部的 connect 函数来使 Redux 正常工作。

还有一个快速介绍使用 Redux 和钩子的方法！

*更多内容尽在*[*plain English . io*](http://plainenglish.io/)