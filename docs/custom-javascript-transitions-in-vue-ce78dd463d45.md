# Vue 中的自定义 JavaScript 过渡

> 原文：<https://medium.com/geekculture/custom-javascript-transitions-in-vue-ce78dd463d45?source=collection_archive---------16----------------------->

## 以及为什么你会想使用它们

![](img/1d1a1e5199f1042f579effe27de8001b.png)

Vue 非常支持动画过渡。这是它优于 React 的最大优势之一。通过自定义 JS 过渡充分利用它！

假设您已经对 Vue 3 有了基本的了解，我将举例说明一个简单的例子，解释为什么您可能想要使用它，并浏览重要的方面。

*(考虑到我通常写的是如何完成某些 UI 效果的逆向工程，这是比较通用和简单的，但是你必须先走才能跑！这将打开一个充满可能性的世界，我将在以后的文章中阐述。)*

# 活生生的例子

看看我用它做的代码沙箱。特别是，该转换是针对一个“折叠”组件的，当您单击标题时，该组件会展开和折叠，以 300 毫秒的动画显示或隐藏内容。

# <transition>组件</transition>

与动画元素的进入和退出相关的一切都是通过 [Vue 的过渡组件发生的。](https://v3.vuejs.org/guide/transitions-enterleave.html#transitioning-single-elements-components)CSS Transition 的 Transition API 非常简洁，在很多情况下，这就是你所需要的。[Vue 团队创造了一个这样的例子](https://codepen.io/team/Vue/pen/3466d06fb252a53c5bc0a0edb0f1588a)。

# 如果我可以使用 CSS，为什么要使用 JavaScript 转场？

JavaScript 打开了一个 CSS 无法单独实现可能性的世界。

想想 Collapser 组件，它有任意大小的内容，但需要有一致的动画持续时间。使用 JavaScript，我可以简单地读取内容的大小，并在动画中使用它，这在 CSS 中是不可能的。使用合适的工具完成工作！

# <transition>使用 JavaScript</transition>

Transition 接受两个重要的事件处理程序。当元素进入 DOM 时触发`enter`事件处理程序，当元素即将离开 DOM 时触发`leave`事件处理程序。

看看代码沙箱中的文件`/src/animations/slide.js`。函数`useSlide`返回两个函数，它们具有`enter`和`leave`事件处理程序所期望的函数定义。

*   `el`是进入或退出的元素，类型为`[HTMLElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement)`。这就是 JavaScript 转换的强大之处— *你可以做任何事情！*(在我的代码中，它读取元素的高度并调用 [Web Animations API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API) 来动画显示其高度的变化)。
*   `done`是当你制作完元素动画后要调用的函数。

整个过程是这样的:

1.  您用函数签名`(el, done)`创建函数，并将其传递到一个`<transition />`组件中的`@enter`和`@exit`。
2.  当在 DOM 中添加或删除`<transition />`的直接子对象时，Vue 会调用您的函数，这些函数会改变`el`，很可能是以一种创建动画的方式。
3.  最终，您的函数调用`done()`，此时 UI 最终确定，直到下一次状态改变。

# 结论

就这么简单！为了避免过于罗嗦，我故意省略了对代码的一些解释，所以我强烈建议查看代码沙盒示例，或者派生并修改它。希望这篇短文和我的例子能帮助新的 Vue 爱好者将他们的 ui 提升到一个新的水平！