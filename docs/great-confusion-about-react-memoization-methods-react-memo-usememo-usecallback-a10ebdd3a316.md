# 关于 React Memoization 方法的巨大困惑:React.memo，useMemo，useCallback

> 原文：<https://medium.com/geekculture/great-confusion-about-react-memoization-methods-react-memo-usememo-usecallback-a10ebdd3a316?source=collection_archive---------0----------------------->

![](img/3f49f97f4f5ec292fe9e8bf698986073.png)

# 简介:

从 React 16.6 开始，React 团队增加了`React.memo()`作为函数的替代。从 React 16.8 开始，React 团队增加了`React Hook`，所以他们增加了两个钩子`useMemo`、`useCallback.`，在 React 中记忆化的使用变得更广泛，在某些情况下也更重要。因此，在本文中，我们将解释记忆化及其不同的方法。

我们将在文章中讨论以下要点:

1-什么是记忆化？

2-做出反应。备忘录

3-做出反应。纯组件

4-使用备忘录

5-使用回拨

我们应该纪念吗

> ***请跟随我*** *过媒* ***，获取下一篇新文章的通知。*** *我也活跃在 Twitter 上*[***@ IbraKirill***](https://twitter.com/IbraKirill)*。*

# 什么是记忆化？

在[计算](https://en.wikipedia.org/wiki/Computing)中，**记忆**是一种[优化](https://en.wikipedia.org/wiki/Optimization_(computer_science))技术，主要用于通过存储昂贵的函数调用的结果并在相同的输入再次出现时返回缓存的结果来加速计算机程序。它是修改软件系统以使其某个方面工作更有效或使用更少资源的过程。

## 反应中的记忆化:

在组件的生命周期中，当进行更新时，React 会重新呈现组件。当 React 检查组件中的任何更改时，它可能会检测到由于 JavaScript 处理相等和浅层比较的方式而导致的非预期或意外的更改。React 应用程序中的这一更改将导致它进行不必要的重新渲染。

此外，如果重新渲染是一项开销很大操作，如长时间的 for 循环，则会影响性能。昂贵的操作在时间、内存或处理上都是昂贵的。除了潜在的技术问题之外，这可能会导致糟糕的用户体验。

如果一个零件重新渲染，它会重新渲染整个组件树。因此，React 发布了 memo 想法来解决这个问题。

# React .备忘录:

它是一个[高阶组件](https://reactjs.org/docs/higher-order-components.html)，是一个性能优化工具，用于功能组件而不是类。如果我们的函数组件在给定相同属性的情况下呈现相同的结果，React 将记忆、跳过组件的呈现，并重用最后呈现的结果。

**例如:**

```
const MyComponent = React.memo(function MyComponent(props) {/* render using props */});
```

React.memo 只检查属性更改。如果 React.memo 中包装的函数组件在其实现中有一个 [useState](https://reactjs.org/docs/hooks-state.html) 、 [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer) 或 [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext) 钩子，当状态或上下文改变时，它仍然会重新呈现。

**重要提示:**

默认情况下，它只会浅浅地比较 props 对象中的复杂对象。如果我们想要控制比较，我们也可以提供一个定制的比较函数作为第二个参数。

```
function MyComponent(props) {/* render using props */}function areEqual(prevProps, nextProps) {/*return true if passing nextProps to render would returnthe same result as passing prevProps to render,otherwise return false*/}export default React.memo(MyComponent, areEqual);
```

该方法仅作为**性能优化而存在。不要依靠它来“阻止”渲染，因为这会导致错误。**

**react . memo 默认情况下使用浅层比较来确定何时重新呈现是有原因的:**这是因为每次我们需要访问某个值时，检查该值是否已被记忆会产生额外的开销，并且该值的数据结构越复杂，开销就越大。

**注**

与类组件上的[shouldComponentUpdate()](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)方法不同，如果属性相等，areEqual 函数返回 true，如果属性不相等，则返回 false。这是 shouldComponentUpdate 的逆操作。

让我们考虑一个简单的例子，我们不使用记忆化，然后我们添加记忆化:

每次点击`inc`时，`renderApp`和`renderList`都会被记录，即使`List`没有任何变化。如果树足够大，它很容易成为性能瓶颈。我们需要减少渲染的数量。

在上面的例子中，**记忆化**工作正常，减少了渲染次数。在安装期间，`renderApp`和`renderList`被记录，但是当`inc`被点击时，只有`renderApp`被记录。

什么时候使用 React.memo:

如果 React 组件:

1-在给定相同属性的情况下，将总是呈现相同的东西(例如，如果我们必须进行网络调用来获取一些数据，并且数据可能不相同，请不要使用它)。

2-渲染开销很大(即，渲染至少需要 100 毫秒)。

3-经常渲染。

如果我们的组件不满足上述要求，那么我们可能不需要像使用 React.memo 那样对其进行记忆，在某些情况下，这会使性能更差，因为需要客户端解析更多的代码。

当有疑问时，[剖析我们的组件](https://reactjs.org/docs/profiler.html),看看记住它是否有益。

# 做出反应。纯组件

我们可以对基于类的组件做同样的事情，像功能组件用`React.memo()`包装功能组件，但是这里我们将在类组件中扩展`PureComponent`。

`React.PureComponent`类似于`[React.Component](https://reactjs.org/docs/react-api.html#reactcomponent)`。两者的区别在于，`[React.Component](https://reactjs.org/docs/react-api.html#reactcomponent)`不实现`[shouldComponentUpdate()](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)`，而`React.PureComponent`用一个浅层的道具和状态比较来实现。

如果我们的 React 组件的`render()`函数在给定相同的属性和状态的情况下呈现相同的结果，我们可以在某些情况下使用`React.PureComponent`来提高性能。

```
class MyComponent extends React.PureComponent {}
```

## 何时使用组件或纯组件:

`React.PureComponent`和`React.Component`的主要区别在于`PureComponent`对状态/道具的**浅层比较**是否改变。这意味着当比较标量值时，它比较它们的值，但是当比较对象时，它只比较引用。它有助于提高应用程序的性能。

当我们满足以下任何一个条件时，我们应该选择`React.PureComponent`。

*   状态/属性应该是不可变的对象
*   状态/属性不应有层次结构
*   当数据改变时，我们应该调用`forceUpdate`

如果我们使用`React.PureComponent`，我们应该确保所有的子组件也是纯的。

# 使用备忘录:

`useMemo()`是一个内置的 React 钩子，接受两个参数——一个计算结果的函数和一个`depedencies`数组。

```
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

在初始渲染期间，`useMemo(compute,dependencies)`调用 compute，存储计算结果，并将其返回给组件。当其中一个依赖关系改变时,`useMemo`只会重新计算记忆值。这种优化有助于避免每次渲染时进行昂贵的计算。如果在下一次渲染中依赖关系没有改变，那么 useMemo() *不会调用* compute，而是返回 memoized 值。

记住传递给`useMemo`的函数在渲染时运行。不要做任何我们在渲染时通常不会做的事情。例如，副作用属于`useEffect`，而不是`useMemo`。

依赖项的作用类似于函数中的参数。依赖列表是`useMemo`观察的元素:如果没有变化，函数结果将保持不变。否则，它将重新运行该函数。如果它们没有改变，即使我们的整个组件重新呈现也没有关系，函数不会重新运行，而是返回存储的结果。如果包装的函数很大且很昂贵，这可能是最佳选择。**这是** `**useMemo**` **的主要用途。**如果我们的函数使用 props 或 states 值，那么一定要将这些值表示为依赖项。如果没有提供数组，每次渲染都会计算一个新值。我们可以依赖`useMemo`作为性能优化，而不是语义保证。

useMemo 试图解决两个问题:引用相等和计算开销大的操作。

将来，React 可能会选择“忘记”一些以前记忆的值，并在下次渲染时重新计算它们，例如，为屏幕外组件释放内存。

按照建议，编写我们的代码，使其在没有`useMemo`的情况下仍能工作——然后添加它以优化性能。

**注意:**依赖项数组不会作为参数传递给函数。从概念上讲，这就是它们所代表的:函数中引用的每个值也应该出现在依赖关系数组中。将来，一个足够先进的编译器可以自动创建这个数组。

我们建议使用`[exhaustive-deps](https://github.com/facebook/react/issues/14920)`规则作为我们`[eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation)`包的一部分。当依赖项指定不正确时，它会发出警告，并提出修复建议。

## 示例:

下面的例子很简单，只是为了理解如何使用`useMemo`:

每次我们改变输入值，计算`factorialResult`，并且`'calculateFactorial(number) called!'`被记录到控制台。

另一方面，每次我们点击增量按钮，`inc`状态值就会更新。更新`inc`状态值触发`<MyComponent />`重新渲染。但是，作为一个次要效果，在重新渲染期间，`calculateFactorial`被重新计算，再次`'calculateFactorial(n) called!'`被记录到控制台。

通过使用`useMemo(()=> calculateFactorial(number), [number])`而不是简单的`calculateFactorial(number)`，React 实现了阶乘计算。

每次我们更改输入值时，`'calculateFactorial(n) called!'`会被记录到控制台，如果我们单击增量按钮，`'calculateFactorial(n) called!'`不会被记录到控制台，因为`useMemo`返回记忆阶乘计算。

我建议欧洲大学 [**在线学位**](https://click.linksynergy.com/fs-bin/click?id=GGg4no0HUcA&offerid=871625.130&subid=0&type=4) 课程的读者，他们中的许多人 [**是免费的。**](https://click.linksynergy.com/fs-bin/click?id=GGg4no0HUcA&offerid=871625.130&subid=0&type=4)

## 大注意:

虽然`useMemo()`可以提高组件的性能，但我们必须确保[在使用和不使用挂钩的情况下对组件](https://reactjs.org/docs/profiler.html)进行仿形。只有在那之后，才能做出记忆是否值得的结论。

当内存化使用不当时，可能会损害性能。

## 为什么 React 的 useMemo 钩子不是所有值计算的默认钩子？

在内部，React 的 useMemo 钩子必须为每次重新渲染比较依赖关系数组中的依赖关系，以决定是否应该重新计算值。通常，这种比较的计算可能比重新计算值更昂贵。

**结论:** `useMemo(() => computation(a, b), [a, b])`是让我们记住昂贵计算的钩子。给定相同的`[a, b]`依赖关系，一旦被内存化，钩子将返回被内存化的值，而不调用`computation(a, b)`。

# **使用回调:**

React 的 useCallback 钩子可以用来优化 React 函数组件的渲染行为。

```
const memoizedCallback = useCallback( () => { doSomething(a, b);},[a, b],);
```

我们先通过一个例子来说明问题，然后用 React 的 useCallback 钩子来解决。注意，这个问题只发生在功能组件上，而不是基于类的组件。

## 示例:

让我们看看下面这个流行的 React 应用程序的例子，它呈现一个用户列表，并允许我们用回调处理程序添加和删除项目。

在输入字段中键入内容以向列表中添加项目应该只会触发应用程序组件的重新呈现，但是当我们键入输入文本时，所有子组件都会重新呈现，请检查 console.log，我们希望防止每个组件在用户在输入字段中键入内容时重新呈现。所以我们会用`React.memo`:

但是，当在输入字段中键入时，两个函数组件仍然会重新呈现。对于输入字段中键入的每个字符，请检查控制台日志:

```
// after typing one character:Render: AppRender: ListRender: ListItemRender: ListItem
```

让我们看看传递给列表组件的[属性](https://www.robinwieruch.de/react-pass-props-to-component):

```
const App = () => {return (*//...*<List *list*={users} *onRemove*={handleRemove} />)
```

只要没有从`list`道具中添加或删除任何项目，它应该保持完整，即使用户在输入字段中键入内容后应用程序组件重新呈现。那么为什么会出现这种行为。

## 为什么会出现这种行为！！！！！

**每当有人在输入字段中键入内容后，应用程序组件重新呈现时，应用程序中的 handleRemove 处理函数都会被重新定义。这是函数组件中纯函数的问题，因为对于类组件，它不做这些行为。**

**通过将这个新的回调处理程序作为一个属性传递给列表组件，它注意到一个属性与之前的呈现相比发生了变化。这就是列表和 ListItem 组件重新呈现的原因。**

我们可以使用 useCallback 来对函数**进行内存化，这意味着只有当依赖数组中的任何依赖发生变化时，该函数才会被重新定义:**

```
const App = () => {...const handleRemove = React.useCallback((id) => setUsers(users.filter((user) => user.id !== id)),[users]);...};
```

如果用户状态通过在列表中添加或删除某个项目而改变，那么处理函数将被重新定义，子组件也将重新呈现。

然而，如果某人只在输入字段中输入，那么函数不会被重新定义并保持不变。

## **为什么 React 的 useCallback 钩子不是 functions 组件内所有纯函数的默认钩子？**

React 的 useCallback 挂钩必须在每次重新渲染时比较依赖关系数组中的依赖关系，以决定是否应该重新定义函数。这种比较的计算通常比仅仅重新定义函数更昂贵。这就是为什么我建议使用 [profiler API](https://reactjs.org/docs/profiler.html) 来检查我们是否应该使用它。

**结论**React 的 useCallback 钩子是用来记忆函数的。当函数被传递给其他组件时，性能已经有了很小的提高，而不用担心父组件每次重新呈现时函数都要重新初始化。正如我们所见，React 的 useCallback 钩子在与 React 的 memo API 一起使用时开始发光。

# 我们应该纪念吗

对于大多数用例来说，反应非常快。如果我们的应用程序足够快，并且没有任何性能问题，那么不使用内存化是可以的。解决想象中的性能问题是一件真实的事情，所以在我们开始优化之前，请确保您熟悉 [React Profiler](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html) 。

React 16.5 增加了对新的 DevTools profiler 插件的支持。这个插件使用 React 的实验分析器 API 来收集每个组件的时间信息，以便识别 React 应用程序中的性能瓶颈。它将完全兼容我们即将推出的[时间片和悬念](https://reactjs.org/blog/2018/03/01/sneak-peek-beyond-react-16.html)功能。

如果我们已经确定了渲染缓慢的场景，那么记忆化可能是最好的选择。

# 结论:

*`useMemo`*用于记忆值，* `React.memo` *用于包装 React 函数组件以防止重新渲染。* `useCallback` *用于记忆功能。我希望如果你喜欢这篇文章并为之喝彩，请跟我来获取关于新文章的通知。**

> *如果你喜欢读这篇文章，并且想支持我成为一名作家，你可以 [**请我喝杯咖啡！**](http://buymeacoffee.com/kirillibrahim)*
> 
> *如果你想深入研究 [**如何在 nodejs/react/javascript 和 V8 引擎中使用 heapdump、快照和 profiler 来检测内存泄漏**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.4210020&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fdetecting-memory-leaks-in-nodejs-and-v8%2F) **s** ，我用下面的 [**课程来建议你。**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.4210020&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fdetecting-memory-leaks-in-nodejs-and-v8%2F)*
> 
> *如果你想 [**学习用 Jest 和 React 测试库测试**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.3780436&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Freact-testing-library%2F) 你的应用的最佳实践，我建议你参加下面的 [**课程。**](https://click.linksynergy.com/link?id=GGg4no0HUcA&offerid=507388.3780436&type=2&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Freact-testing-library%2F)*

## *反应 18:*

*[](/geekculture/the-foundational-update-to-core-rendering-model-react-18-610d0de81336) [## 核心渲染模型 React 18 的基础更新

### 简介:

medium.com](/geekculture/the-foundational-update-to-core-rendering-model-react-18-610d0de81336)*