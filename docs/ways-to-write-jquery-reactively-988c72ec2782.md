# 被动编写 JQuery 的方法

> 原文：<https://medium.com/geekculture/ways-to-write-jquery-reactively-988c72ec2782?source=collection_archive---------13----------------------->

![](img/acd5d33eec9ed423d6774f05da045c13.png)

Photo by [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

即使现在是 2021 年，前端框架的三大支柱几乎已经主导了前端开发工具链。但是仍然有一些公司或个人因为一些原因仍然坚持使用 JQuery。

如果您曾经使用 JQuery 来构建您的前端，您可能以前遇到过这个问题。

version with duplicate UI logic

在上面的 gist 中显示了一个使用 JQuery 隐藏一些 HTML 元素的例子，并且相同的逻辑在同一个文件中是重复的。随着代码库变得越来越大，这将使代码成为需要维护的头文件。您需要搜索所有的代码来修改这段逻辑。

> 我们能做得更好吗？
> 
> 答案是肯定的。

我们可以将这个逻辑提取到一个函数中，并像下面的代码一样重用它。

better version with centralized UI logic

在这个版本中，我们通过将 show HTML 元素逻辑放在一起，改进了代码的可维护性。但是当显示/隐藏元素时，我们仍然需要调用 toggleShowSomeElement 函数。

> 我们能做得更好吗？
> 
> 答案是肯定的。

> UI = fn(状态)

现代前端框架认为 UI 是状态的函数。这意味着 UI 应该反映状态，如果状态改变了，那么 UI 也会改变。

其中一些框架使用反应式编程模式来实现这一点。当状态改变时，它会通知用户界面进行自我更新。

将此功能放入我们的 JQuery 代码将完成以下工作:

1.  使用 JQuery 内置函数实现发布-订阅模式。让我们的用户界面在状态改变时自我更新。
2.  添加一些自定义 HTML 属性，将状态绑定到 HTML 元素中。
3.  将所有事件监听器绑定到一起。
4.  用初始状态初始化用户界面。

这就是当我们完成所有工作时代码的工作方式。通过这样做，我们可以通过订阅状态更改来更改状态以更新 UI。

让我们一步一步地实现这个功能:

## 1.使用 JQuery 内置函数实现发布-订阅模式。让我们的用户界面在状态改变时自我更新。

在本节中，我们将使用杠杆 [**。JQuery 中的 trigger()**](https://api.jquery.com/trigger/) 函数在任何状态改变时发布事件。我们将创建一个名为**的函数。**

updateState

## **2。添加一些自定义 HTML 属性，将状态绑定到 HTML 元素中。**

在这个 html 文件中，我们添加了一些自定义 HTML 属性:

1.  数据键:状态的键，我们将使用它来绑定状态和元素。
2.  数据-文本:当状态改变时，具有该属性的 HMTL 元素将更新值/文本。
3.  data-class+data-toggle:data-class 属性将让我们的代码知道我们想要在它上面切换类，data-toggle 将定义当状态为 true/false 时您想要切换的类；
4.  data-attr+ data-toggle:和上面那个一样，data-attr 属性会让我们的代码知道我们要在它上面 toggle attribute，data-toggle 会定义状态为 true/false 时你要切换的一个/多个属性；

我们完成了 HTML 部分，让我们添加一些逻辑来用这些 Html 属性更新 UI！

在这一节中，我们将接收已经更新的状态的键和值，然后我们将状态更新到用自定义属性标记的相应 HTML 元素上。

## 3.将所有事件监听器绑定到一起。

在本节中，我们将使用 **$(state)绑定按钮事件并添加订阅状态更改。on()** 并将回调函数 **updateElement()** 附加到它，以确保 UI 会在状态改变后更新。

## 4.用初始状态初始化用户界面。

在这一节中，当 dom 准备好时，我们通过调用 **init()** 和 **bindEvents()** 来总结我们在上一步中完成的所有事情(只是为了确保元素被呈现)。

这就是利用现代前端框架的概念编写 JQuery 的全部内容，感谢阅读！