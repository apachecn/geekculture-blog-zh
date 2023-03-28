# 无需额外的库，在 React 中为您的网站重新创建页面堆栈

> 原文：<https://medium.com/geekculture/recreating-page-piling-in-react-for-your-personal-sites-without-extra-libraries-23fcde868ed9?source=collection_archive---------18----------------------->

## 在个人网站上使用的奇特效果

最近偶然发现一个以前同学的个人网站。她的网站让我大吃一惊，因为我对设计完全没有眼光。我所拥有的是一颗好奇的心和建立好想法的意愿。一个特别好的想法是这种过渡效应:

![](img/dbf29a1003468c0520f27281ee67920f.png)

A screencap of [https://w3hubs.com/Full-Screen-Scrolling-Using-Page-Piling-js/](https://w3hubs.com/Full-Screen-Scrolling-Using-Page-Piling-js/); I avoided using my former classmate’s site to preserve her anonymity

顺便说一下，她使用的是 [pagePiling.js](https://alvarotrigo.com/pagePiling/) jQuery 插件。

我喜欢。让我们只用 React 库和 web APIs 自己重新创建它。

为什么要重新创建而不是使用插件？混合使用 jQuery 和 React 是一件麻烦的事情。另外，我喜欢挑战。也许我们会学到一些东西。

# 要求

你可以在 [pagePiling.js](https://alvarotrigo.com/pagePiling/) library 页面自己摆弄它。由于 pagePiling.js 是一个开源的、由社区维护的库，并且我正在编写一个周末项目，所以我将满足以下最低要求:

*   将有一个 React 组件，它接受一个页面部分列表作为子页面
*   该组件一次显示一个部分
*   滚动过页面的顶部或底部会导航到下一部分
*   过渡到上一节会向下滑动该节
*   过渡到后面的部分会向上移动该部分
*   应该有一种方法来更新 URL，这样刷新页面应该会将您返回到该部分(即，它默认为 URL 中指定的部分)

# 密码

查看以下内容:

*   [现场](https://ww-page-piling.netlify.app/)
*   [回购](https://github.com/weimingw/weiming-page-piling)
*   [组件代码](https://github.com/weimingw/weiming-page-piling/blob/Component/src/components/PagePiling.js)

# 说明

该组件可分为以下几部分:

*   绝对定位和`min-width`和`min-height`使得单个页面部分覆盖整个视窗，这意味着我们一次只显示单个部分。
*   `Transition`来自 [react-transition-group](https://github.com/reactjs/react-transition-group) 库的组件，结合 [Web 动画 API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API) ，形成上下移动页面的动画。(这不可能使用 CSS 过渡，因为动画会根据页面的变化而有所不同。)这些`Transition`组件包围了`PagePiling` 组件的每个子组件。
*   为了避免页面动画过渡时的额外变化，一个额外的`transitioning`布尔值跟踪它是否在变化，在动画开始前设置为 true，当动画结束时设置为 false。
*   每个部分跟踪两类指示滚动的事件——桌面用户的滚轮事件和移动用户的触摸事件。触摸是`touchstart`和`touchmove` — `touchmove`的组合，以跟踪滚动是向上还是向下。
*   该组件跟踪当前查看的部分和先前查看的部分。由此，它可以判断页面是向上导航还是向下导航。这个方向被传递给`PagePilingSection`组件，组件用它来决定播放哪个动画。
*   一个简单的回调`onViewChange`在节发生变化时触发，一个`initialView` prop，结合对 URL 的读写，让组件在刷新时保持其状态。

# 结论

老实说，“堆页”并不是一个难以实现的效果。对大多数人来说，将`react-transition-group`与网页动画 API 混合可能是最大的障碍。在此基础上，我们还可以做很多进一步的改进，所以可以自由地按原样派生或复制代码。作为一次学习经历，我从这个项目中发现了`wheel`事件，我希望代码或这篇文章也能对您有所帮助！