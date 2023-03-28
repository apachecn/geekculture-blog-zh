# 反应使用状态:概述

> 原文：<https://medium.com/geekculture/react-usestate-a-recap-79552ee35e2d?source=collection_archive---------19----------------------->

![](img/8933e4fa3854e49902a096a7f2866351.png)

React js

在过去的几个月里，我一直在钻研 **React** 并构建了一些简单的应用程序。我一直很难理解的一个概念是**反应状态**。我最近偶然发现了一个很好的[教程](https://www.youtube.com/watch?v=4pO-HcG2igk&list=PL4cUxeGkcC9gZD-Tvwfod2gaISzfRiP9d&index=8)(我强烈推荐)很清楚地解释了这个问题。在这篇简短的文章中，我将尝试总结我在跟随教程的过程中学到的最重要的概念，以便对其他人也有所帮助:-)

## React 中的状态是什么？组件的状态是在特定时间在我们的组件中使用和呈现的数据。

在下面的代码中，我们创建了一个变量(iceCreamFlavor ),并赋予它一个初始值“chocolate”。这个变量在我们的组件模板中使用花括号{iceCreamFlavor}呈现。我们希望通过点击“点击我”按钮，冰淇淋的口味会从“巧克力”变成“草莓”…毕竟这就是 handleClick 功能的作用…

```
const MyComponent = () => {let iceCreamFlavor = "chocolate";const handleClick = () => {
iceCreamFlavor = "strawberry";
};return (
<div className="container-style">
<h2>Ice cream flavor</h2>
<p>{iceCreamFlavor}</p>
<button onClick={handleClick}>Click me</button>
</div>
);
};
```

而且……没有。当我们点击时，什么也没有发生。有趣的是，如果我们在点击按钮后登录 iceCreamFlavor，值确实从“巧克力”变成了“草莓”,但只是在控制台中。

该值会更改，但不会呈现给组件。为什么会这样呢？

**这是因为 iceCreamFlavor 不是一个反应变量，因此它的变化不会触发 React 来重新呈现组件模板。**

**为了使值具有反应性，我们需要使用 useState 钩子，这是一种特殊类型的函数。**

通过写:

```
useState("chocolate")
```

我们可以让“巧克力”这个值起反应。

“chocolate”是使用花括号在组件内部呈现的初始值。但是钩子还没有完成。为了最终完成，我们还需要:

1.  存储该值的“位置”。在这种情况下，我们将使用“iceCreamFlavor”(注意，这个变量名是任意的)。
2.  负责更新值“iceCreamFlavor”的函数，在本例中为“setIceCreamFlavor”。通常函数名以“set”开头，后面是我们要更新的内容的名称。

```
const [iceCreamFlavor, setIceCreamFlavor] = useState(“chocolate”)
```

请注意，在开始时，当我们第一次呈现组件时，iceCreamFlavor 变量的值是“chocolate”。 **setIceCreamFlavor 触发模板重新呈现，允许用户看到更新后的值。**

如果我们将挂钩添加到代码中，最终的代码将如下所示:

```
const Home = () => {
  const [iceCreamFlavor, setIceCreamFlavor] = useState(“chocolate”)

  const handleClick = () => {
    setIceCreamFlavor("strawberry");
  };return (
 <div className="container-style">
 <h2>Ice cream flavor</h2>
 <p>{iceCreamFlavor}</p>
 <button onClick={handleClick}>Click me</button>
 </div>
 );
};
```

当我们点击“点击我”按钮时:

1.  handleClick 函数被激发。
2.  setIceCreamFlavor 也被触发，这将把冰淇淋的味道从“巧克力”改为“草莓”,同时也触发了模板的重新渲染。

我们现在可以看到，浏览器中的冰淇淋口味已经发生了变化。！

关于这个钩子的几个旁注:

*   在一个组件中，我们可以任意多次使用 useState。
*   为了使用这个钩子，需要在组件的开头导入它，如下所示:

```
import { useState } from “react”;
```

useState 的另一个用例可能是，例如，每当我们想从页面中删除一个元素时。假设我们触发一个函数从列表中删除一个项目，在这种情况下，我们需要将该值设置为 reactive，这样 React 将重新呈现包含我们的列表的页面，而不显示我们删除的项目。

我希望这篇文章能帮助你更好地理解使用状态的概念。今后，随着我作为前端开发人员的职业生涯的进展，我会努力写更多这样的文章:-)。