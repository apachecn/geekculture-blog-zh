# 理解 React 挂钩—第 1 部分

> 原文：<https://medium.com/geekculture/understanding-react-hooks-part-1-55fe274d6d3d?source=collection_archive---------16----------------------->

ReactJS 在其 16.8 版本中发布了一个主要特性，其中功能组件的行为将更像一个类组件。如果您担心无法在功能组件中使用组件状态或生命周期方法，现在您可以使用 react 挂钩来实现。

![](img/5b87113d165a41416cc3828cb1cec047.png)

react 钩子的目标是尽可能快地覆盖类的所有用例，然而，目前还没有相当于不常见的`getSnapshotBeforeUpdate`、`getDerivedStateFromError`和`componentDidCatch`生命周期的钩子，但也许在未来的版本中会有？

在我们深入研究各种 react 挂钩之前，有几个常见问题。

***问:React 的哪些版本包含钩子？***

A.从 16.8.0 开始，React 包含了 React 钩子的稳定实现。

问:钩子覆盖了类的所有用例吗？

A.不，正如上一段提到的，还没有不常见的`getSnapshotBeforeUpdate`、`getDerivedStateFromError`和`componentDidCatch`生命周期的挂钩等价物，它可能会在未来的版本中出现。

问:如何使用钩子获取数据？

A.`useEffect`可以用来获得一个类组件的生命周期效果，这个马上就要讲到了。

问:我需要重写我所有的类组件吗？

A.不，类组件仍然存在，我们应该开始对项目中的所有新组件使用钩子。

让我们学习一些 react 挂钩，并与这些有趣的概念挂钩，看看我们如何使用它们。

我们将在本文中讨论`useState`、`useEffect`和`useRef`，并最终在以后的文章中学习许多其他钩子。

让我们开始吧…

1.  `useState`

顾名思义，它将允许您维护功能组件的本地状态。

***语法*** 、

`const [stateVariableOrObject, updateStateFn] = useState(initailState);`

***用法*** :

```
import React, { useState } from 'react';
const [name, updateName] = useState('medium');
```

在上面的例子中，`name`是一个状态变量，`updateName`是一个更新值的函数。每当有更新时，组件就会显示更新后的状态，而`useState`是一个反应钩子。此外，带有字符串`medium`的`useState`是状态变量`name`的初始值。

您也可以创建状态对象。这里有一个例子，

`const [state, updateState] = useState({name: 'medium', state: 'AZ'});`

然后像这样使用状态变量`const { name } = this.state;`

一个代码示例。

如上例所示，`name`将把`medium`作为它的初始值，一旦点击按钮，它将调用`updateName`函数并获得一个新值`Name Updated`

**注**:

*   如果需要，可以在一个组件中使用多个`useState` react 钩子。通过使用状态对象而不是单个状态变量，基本上可以实现相同的效果。然而，使用多个`useState`不会让您考虑状态对象中的其他状态属性。

2.`useEffect`

我们看到了如何使用`useState`和维护功能组件的本地状态，但是像类组件那样的生命周期钩子又是怎么回事呢？当组件安装或更新时，我们如何执行副作用？这就是`useEffect`出现的原因。`useEffect`钩子帮助我们在每次功能组件渲染时执行一个动作。

***用法*** :

```
import React, { useEffect } from 'react';useEffect(() => {
    . . .
});
```

*   **当你想在每次组件渲染时执行一个动作**

在上面的例子中，控制台上的`Hey there !!!`将在您每次点击`update`按钮时被记录，这意味着每当状态改变时，组件重新渲染并且`useEffect`将被触发。

*   **当您想要对组件更新执行操作时**

`useEffect`接受第二个参数，这将有助于组件更新生命周期仅在特定状态更新时运行特定操作。

在上面的代码示例中，控制台日志`Only trigger on name update`只发生两次，一次是当组件以值`hello`呈现时，另一次是当您单击`Update`按钮时。由于`name`的值现在将是`world`，如果我们继续单击`update`按钮，`count`状态变量会增加，但是`useEffect`不会每次都触发。

*   **当您想只在组件支架上执行动作时**

要在组件挂载中触发`useEffect`，只需传递第二个参数作为空数组`[]`。因为没有提到状态，所以这将在组件呈现时触发。

*   **当您希望仅在组件卸载时执行操作**

如果你想清理一个组件，你可以通过在一个`useEffect`函数中编写一个`return () => {}`函数来完成，这个返回语句将会起到清理的作用。每当触发`useEffect`时，它将首先运行返回功能，查看是否需要清理，然后运行返回功能之外的步骤。

在上面的例子中，当您点击`update`按钮时，将首先记录`unMount`，然后记录`onMount`。您可以使用这种清理来清除任何事件侦听器(如果有)，或者取消订阅任何订阅。

3.`useRef`

顾名思义`useRef`可以用来引用或访问子 HTML 组件/DOM 元素。`useRef`返回一个可变的 ref 对象，其`.current`属性被初始化为传递的`initialValue`。返回的对象将在组件的整个生存期内保持不变，并且不会在组件发生更改时重新呈现组件。

***用法:***

```
import React, { useRef } from 'react';export const ComponentUseRef() {
    const inputRef = useRef(null);return <>
        <input ref={inputRef}/>
        </>
}
```

很少有好的`useRef`用例包括关注一个 HTML 元素或者计算一个组件中发生的渲染次数。

让我们看看这两个例子，以便更好地理解它们。

*   聚焦于输入元素

在上面的例子中，输入元素是用`useRef`引用的，这里的`inputref.current`是指`<input/>`元素。所以每当点击`Focus the input`按钮，就会调用聚焦在输入栏的`onButtonClick`函数。

*   记录组件中发生的渲染次数。

让我们尝试使用到目前为止我们所学过的所有钩子，`useState`保存输入值并更新输入值，`useEffect`更新渲染计数，`useRef`实例化`renderCount`在组件的整个生命周期中持续存在的值。

在上面的代码示例中，每当`name`状态值更新时，它就会呈现组件，从而增加`renderCount`值，这里可能会有一个问题/困惑，为什么我们应该使用`useRef`对吗？我们可以用另一个状态变量`renderCount`？？但问题是，这将导致一个无限循环，因为我们更新了名称，这实际上更新了`renderCount`，这将再次呈现组件，这里需要注意的重要一点是`useRef`不会导致重新呈现组件。

`useRef`的另一个重要性是存储/获取 prevProps 或 prevState。

查看 [GitHub](https://github.com/pnaika/react-hooks) 中的一些例子

在接下来的文章中，我们将看到一些例子并了解`useReducer`、`useMemo`和`useCallback`

**参考**

1.  [https://reactjs.org/docs/hooks-intro.html](https://reactjs.org/docs/hooks-intro.html)
2.  [https://reactjs.org/docs/hooks-faq.html](https://reactjs.org/docs/hooks-faq.html)

***在这里了解我，***

[www.prashanthpnaika.com](http://www.prashanthpnaika.com/)

[https://www.linkedin.com/in/prashanthnaika](https://www.linkedin.com/in/prashanthnaika)

h[ttps://github . com/pn aika](https://github.com/pnaika)