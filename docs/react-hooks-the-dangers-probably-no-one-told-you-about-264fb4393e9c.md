# 反应钩子:可能没人告诉你的危险

> 原文：<https://medium.com/geekculture/react-hooks-the-dangers-probably-no-one-told-you-about-264fb4393e9c?source=collection_archive---------2----------------------->

## 探索钩子带来的危险以及如何保持领先并避免它们。

![](img/c11ea0c50f6f441aad2664f9db2b090e.png)

Photo by [Oscar Omondi](https://unsplash.com/@xodusdigitals?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/mentorship?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

自从 React 16.8 引入钩子以来，几年过去了，大多数开发者已经抛弃了旧的类结构。

> 钩子使得提取逻辑变得容易，也是重用逻辑的一种简单方式。在最初发布后，他们席卷了前端世界。

在过去的几年里，我在自己的和审查别人的代码中发现了十几个与钩子相关的问题。我试着分享一下我使用钩子遇到的危险，这样你就不用犯同样的错误了。

我们经常认为主流创新是理所当然的，很少对它们进行足够的批判。我相信新的工具也应该用引入新的 bug 或者创造坏的编码习惯的容易程度来衡量。

不要误解我——钩子是老派 React 所缺乏的一切，因为没有简单的方法来提取和共享组件之间的逻辑。开发人员必须处理 HOC(高阶组件)或渲染道具。这两种模式对大多数人来说都太难使用了。

> 如果你对钩子一无所知或者需要复习，这里有一个不错的。[丹·阿布拉莫夫——理解反应钩](/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)

# 1.属国

字符串、布尔值、数字和其他原语可以直接作为依赖项添加到`useEffect`中。但是，您不能将数组和对象用作依赖项。原因——从简单的角度来看，它们没有可比性。

> **默认情况下，数组和对象会通过引用进行比较。**

在以下两种情况下，这些依赖关系可能会影响 useEffect 挂钩的执行:

*   数组或对象是相同的- JS 将使用不同的引用来比较它们
*   数组或对象有不同的值- JS 将使用相同的引用来比较它们。

在这两种情况下，挂钩都会导致 bug，它们不会正确执行。

有一些已知的方法和技巧可以解决这个问题。这些都不是完美的解决方案，但有时我们只需要让事情运转起来。

*   **第一个选项是使用** `**JSON.stringify()**`

*   **另一个(ES6)选项是使用**[**t**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)**emplate 文字将它们转换成字符串。**类似于`JSON.stringify()`，除了结果不会被包在数组里。

*   **第三个选项，如果数组大小不变，将使用扩展操作符:**

# 2.条件式

> 钩子依赖于执行的顺序，所以不可能有条件地调用钩子。

有条件的使用钩子会改变顺序，你的 Linter 会抱怨。这个问题减少了我们构造钩子的方法。

从技术上讲，你不能在条件句中使用钩子。然而，如果你知道它们内部是如何工作的，你可以让条件钩子为你工作。

您需要为要渲染的每个钩子创建两个或更多组件。在每个组件中，您可以添加其逻辑。

The logic for conditional rendering

Hook that will be conditionally rendered.

这样，我们可以基于`userId`变量呈现两个不同的钩子。

# 3.使用效果问题

开发人员使用`useEffect`来修改状态、DOM 或进行 API 调用。

> 我甚至想不出有多少次有人问我，“为什么 useEffect 被调用两次？”或者“为什么我在 useEffect 中有一个无限循环？”。

那么为什么 useEffect 首先是不好的呢？

*   useEffect 的好处是:你几乎可以用它做任何事情
*   useEffect 的缺点是:你几乎可以用它做任何事情

## 无限循环问题

> [https://stack overflow . com/questions/53070970/infinite-loop-in-use effect](https://stackoverflow.com/questions/53070970/infinite-loop-in-useeffect)——这个问题并不是唯一的一个——在 11 个月前添加，被 17.5 万开发者看到。

完成无限循环最直接的方法是当一些状态改变时触发效果，当它改变时，您运行代码来触发这个确切的状态改变。

最快的解决方案是删除导致此问题的依赖关系。

你应该始终使用多重使用效果来保持单一责任原则。依赖越少，潜在的错误就越少。

这个钩子中的代码太多可能会给你带来麻烦。花些时间提取并重构`useEffect`中的函数。

# 4.过度使用 useCallback & useMemo

新的 React 开发人员通常认为这是一种更安全的记忆函数的方法。需要时，记忆化提供了一套强大的工具来防止不必要的渲染。

然而，太好的东西都有很高的价格。通过使用 useCallback 和 useMemo，我们增加了代码的复杂性，这会影响开发的速度，并且从长远来看会引入错误。

> 性能优化不是免费的。我们应该把 useMemo 和 useCallback 看作是对 app 的微调。

## 何时不优化

尽管它们功能强大，但你不应该对每个函数都使用 useMemo 或 useCallback。使用前，考虑先检查 app 性能。在添加这些挂钩之前，开始改进您的代码通常更好。

大多数时候，开发人员不应该费心去优化不必要的重新呈现器。天生反应快；我不认为我在使用它时遇到过性能问题。—对于 React 原生和移动应用程序开发来说，情况并非如此。

这两者不能修复有害的代码，但是如果一切都已经就绪，它可以让你的代码变得更好——很好。

## 最好进行优化的情况

当处理输入逐渐变化的函数时，可以使用`useMemo`。此外，当数据大到足以导致内存问题时，或者当参数大到比较的成本不会超过包装器的使用时。

`useCallback`在每次调用都要重新编译代码的情况下工作良好。当输入随时间变化时，记忆结果可以减少反复调用函数的成本。

**用 useMemo 进行昂贵的计算**

`useMemo`的好处是你可以得到这样一个值:

```
const a1 = {b: props.b}
```

懒洋洋地得到它:

```
const a2 = useMemo(() => ({b: props.b}), [props.b])
```

在这种情况下`useMemo`是没有用的，但是想象一个计算一个值的函数，这个值的计算是非常昂贵的。不是很多应用程序都这样做，但如果他们这样做了，useMemo 是一个完美的解决方案。

通常，如果你开始注意到渲染问题，这是优化重新渲染的最佳时机。

编码快乐！✋