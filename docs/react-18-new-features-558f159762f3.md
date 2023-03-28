# React 18 项新功能

> 原文：<https://medium.com/geekculture/react-18-new-features-558f159762f3?source=collection_archive---------8----------------------->

![](img/4f5a39dea194499e1105bede8f252992.png)

*React 17 专注于改善基础，但 React 18 中增加了一些重要的东西。在这篇文章中，我们将浏览一些关于如何开始使用 react 18 alpha 中很酷的新功能的最新更新。*

安装 React 18 alpha : `npm install react@alpha react-dom@alpha`

下面是几个更新的列表。

1.  新建根 API
2.  焦虑
3.  暂停列表
4.  useDeferredValue
5.  自动配料

> ***新根 API***

在 React 18 中有一个新的根 API。

在前面的 reactDOM.render 方法中，我们使用传递 App 组件，然后是 document.getElementById 和根元素。因此，我们将应用程序组件呈现到页面上的根元素中。

```
import ReactDOM from "react-dom";
import App from "App";
ReactDOM.render(<App />, document.getElementById("root"));
```

在 React 18 中，我们首先必须通过 createRoot 方法创建根。这将被传递给我们的根元素，然后我们调用 *root.render* 并传递我们的应用程序组件。

```
import ReactDOM from "react-dom";
import App from "App";
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

这里，我们的 React 应用程序的根已经被分离。我们现在首先需要使用 createRoot 方法创建根，然后在其上调用 render 方法。

> ***悬念***

就像它的名字一样，悬疑暂停某些东西，直到它准备好被渲染。

在 react 的以前版本中，下面的代码会导致 ReadyComponent 被立即挂载，并调用它的效果。

```
<Suspense fallback={<Loading />}>
    <ComponentWaitingForData />
    <ReadyComponent />
</Suspense>
```

这在 React 18 中已经解决了。

参考上面的代码，现在它不会挂载 ReadyComponent，而是先等待 ComponentWaitingForData 进行解析。

> ***暂停***

一个悬而未决的人期待两个道具。“revealOrder”和“tail”。

“revealOrder”是暂停列表配置选项之一。它可以是未定义的、一起的、向前的和向后的。

*   *undefined* (默认):当吊杆解决时显示孩子。
*   *一起*:一旦所有的吊杆都解决了，一起露出孩子。
*   *转发*:从上到下渲染小孩，对吊带无动于衷。决议顺序
*   *向后*:自下而上渲染子体，不考虑吊杆分辨率顺序

“tail”属性决定了悬挂列表中未加载的项目是如何显示的。它的值可以折叠或隐藏。

```
<SuspenseList revealOrder="forwards" >
    <Suspense fallback={<p>Loading attendance...</p>}>
        <Attendance id={facultyID}/>
    </Suspense>
    <Suspense fallback={<p>Loading homework...</p>}>
        <Homework id={facultyID}/>
    </Suspense>
</SuspenseList>
```

上面带有 SuspenseList 的代码演示了我们可以设置 revealOrder 来强制出勤先出现，然后是作业部分。

```
<SuspenseList revealOrder="forwards" tail="collapsed">
    <Suspense fallback={<p>Loading attendance...</p>}>
        <Attendance id={facultyID}/>
    </Suspense>
    <Suspense fallback={<p>Loading homework...</p>}>
        <Homework id={facultyID}/>
    </Suspense>
</SuspenseList>
```

上面的代码演示了一次只显示一个回退。即首先是出勤的后退，然后是家庭作业的后退。

> ***useDeferredValue***

“useDeferredValue”是一个挂钩，它将返回传递值的延迟版本。它接受状态值和以毫秒为单位的超时。它将返回该值的延迟版本，对于大多数超时，该值可能“滞后于”它。

```
import { useDeferredValue } from "react";
const [text, setText] = useState("react js");
const deferredText = useDeferredValue(text, { timeoutMs: 2000 });
```

当我们有一些基于用户输入立即呈现的内容和一些需要等待数据获取的内容时，这通常用于保持 UI 的响应性。

> ***自动配料***

自动配料有了巨大的改进。在 React 的早期版本中，它使用将多个状态更新批处理为一个状态更新，以减少不必要的重新渲染。问题是它只能在 DOM 事件处理程序中完成，而不能在承诺、超时或其他处理程序中完成。

让我们看看下面的代码，看看早期版本的 React 是如何进行批处理的。

```
export default function App() {
    const [count, setCount] = useState(0);
    const [color, setColor] = useState(undefined);

    const handleClick = () => {
        setCount(count + 1); //No re-render
        setColor(count % 2 === 0 ? "Green" : "Red"); //No re-render
        // Now re-renders once at the end (this is batching)
    }     return (
        <>
            <button onClick={handleClick}>Next</button>
            <span style={{
                color: count % 2 === 0 ? "red" : "green",
            }}>
                {color}
            </span>
        </>
    );
}
```

使用 React 18，承诺、超时或其他处理程序也将利用这一点。无论状态更新发生在哪里，它都会批量更新。这将导致更好的性能。

让我们看看下面发生批处理的代码。在下面的代码中，批处理在 React 18 中运行良好，但是早期版本的 React 不能批处理它。

```
export default function App() {
    const [count, setCount] = useState(0);
    const [color, setColor] = useState(undefined);    const handleClick = () => {
        Promise.resolve().then(() => {
            setCount(count + 1); //Re-render
            setColor(count % 2 === 0 ? "Green" : "Red"); //Re-render
        });
    }    return (
        <>
            <button onClick={handleClick}>Next</button>
            <span style={{
                color: count % 2 === 0 ? "red" : "green",
            }}>
                {color}
            </span>
        </>
    );
}
```

然而，如果我们不希望我们的组件被批处理，我们可以使用`ReactDOM.flushSync()`选择退出那个组件。

参考—[https://react js . org/blog/2021/06/08/the-plan-for-react-18 . html](https://reactjs.org/blog/2021/06/08/the-plan-for-react-18.html)

# 结束语:

我们刚刚了解了 React 18 alpha 版本中一些很酷的更新。作为一名 JavaScript 开发人员，我真的很喜欢这些更新，并且很高兴看到 React 18 的测试版是什么样子。

谢谢你一直坚持到最后🙌。如果您喜欢这篇文章或学到了新东西，请点击下面的分享按钮来支持我，与更多人联系，和/或在[*Twitter*](https://twitter.com/amir__mustafa)*上关注我，以查看我在那里学到和分享的其他技巧、文章和东西。*