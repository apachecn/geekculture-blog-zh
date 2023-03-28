# 反应钩子:使用效果

> 原文：<https://medium.com/geekculture/react-hook-useeffect-217b45d3d22a?source=collection_archive---------52----------------------->

useEffect 挂钩非常有用。这是不久前发布的另一个便利特性，名为 React Hook，是一种管理功能组件内部的组件生命周期和应用程序状态的方法。

useEffect 钩子实质上替换了类组件中的 componentDidMount 生命周期函数，但是 useEffect 用于功能组件中。

UseEffect 最常用于设置组件的状态、获取数据、读取或写入本地存储，或者设置事件侦听器。

主要功能是 useEffect 将在组件的初始呈现时发生，并允许更改来重新呈现该组件(要监视更改的项目必须包含在依赖数组中，这将在后面讨论)。

![](img/a7d059194af7c4c13a425836e233f2c2.png)

# 语法

首先，我们需要导入钩子:

```
import {useEffect} from "react"
```

然后，在功能组件内部，使用 Effect 来获取数据或设置事件侦听器。

```
useEffect(() => {
    *fetch data, set event listeners, etc*
}, [optional dependency array])
```

# 依赖关系…

依赖数组是可选的，但通常被设置为一个项目列表，可以是函数、变量或状态，它们将被监视是否有变化。如果其中一项发生变化，组件将被重新呈现。如果依赖项数组为空，useEffect 将只运行一次，即在组件的初始呈现时。

我强烈推荐您非常熟悉这个钩子，因为这是一个控制副作用的惊人方法，它从我们的老朋友“componentDidMount”的一个类中获得了额外的抽象层次。

这是一个深入研究 useEffect 钩子以及其他一些通常与它结合使用的钩子的惊人资源:[你需要的 useEffect 钩子的最后指南！](https://blog.logrocket.com/guide-to-react-useeffect-hook/)

干杯，和一些副作用玩得开心！