# 使用 MutationObserver 管理多个反应根

> 原文：<https://medium.com/geekculture/managing-multiple-react-roots-using-mutationobserver-93d35f070d54?source=collection_archive---------21----------------------->

![](img/0d0c7058d7cb2c19c5b784d084300d03.png)

如果要将 React 添加到现有的 web 应用程序中，很可能需要多次使用 ReactDOM.render()。如果它在同一个屏幕上，这应该不是问题，但是如果 HTML 元素在您切换视图时来来去去，您将需要使用 index.js 文件中定义的函数分别处理它们。

根据 React 文档，使用多个根来渲染组件树是完全没问题的，事实上，这就是脸书使用它的方式。

在本例中，当在另一个文件中定义的组件中单击按钮时，我想调用 index.js 文件中定义的函数。我将使用数据属性，React useEffect 钩子，和变异观测器。

**数据属性**

首先，我想将目标屏幕的名称存储在一个 DOM 元素中，这样我就可以从应用程序的任何地方访问它。[以 data-*开头的数据属性](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)非常适合这个目的，所以下面的标签将被添加到 html 文件中:

```
<input type="hidden" id="nextscreen" data-screen="nochange" />
```

现在，我可以使用数据集属性访问和修改“属性变量”*屏幕*:

```
document.getElementById("nextscreen").dataset.screen
```

**反应使用效果挂钩**

如果单击了导航按钮，则需要修改数据属性。由于包含元素的数据不是使用 React 创建的，所以需要使用 [useEffect](https://reactjs.org/docs/hooks-effect.html) 钩子，因为我们正在 React 的“外部”进行修改。

我们需要用*使用状态*初始化一个变量，以便能够在*使用效果*中使用它:它是*新闻屏幕*。

```
function navButton(props){ const [ newscreen, setScreen ] = useState("nochange"); const doSwitch = ()=>{ setScreen(props.target); } useEffect(()=>{ document.getElementById('nxtscreen').dataset.screen = newscreen; console.log("Target screen is: "+newscreen); },[newscreen]); return ( <li className="nav-item"> <span className="nav-link" onClick={doSwitch}>{props.btxt}      </span> </li> );}
```

值“nochange”将在第一次渲染时赋值，如果单击该按钮，将调用函数 doSwitch()，将 *props.target* 的值赋给 *newscreen* ，并将其写入隐藏的输入元素。

**变异观察者**

最后，我需要在数据属性更新时执行一些代码。

MutationObserver 允许监听目标元素中的变化，而不会导致性能问题，而且大多数浏览器都支持它。

当观察者在我们的目标元素中检测到属性变化时，回调函数 *navSwitch* ()被调用，现在可以执行期望的代码了。

```
let observer = new MutationObserver(navSwitch);function navSwitch(mutations){ console.log(mutations); // code to switch screen}observer.observe( document.getElementById('nxtscreen'), { attributes: true } );
```

我希望这是有帮助的，让我知道你是否有更好的想法来达到同样的结果！