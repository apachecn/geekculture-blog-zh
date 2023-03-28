# 引擎盖下:反应路由器

> 原文：<https://medium.com/geekculture/under-the-hood-react-router-466365fee324?source=collection_archive---------4----------------------->

## 用不到 40 行代码实现它

![](img/eec6b5efc15fcd0d80991951ce64e780.png)

Photo by [ian dooley](https://unsplash.com/@sadswim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你开发过 React 应用，你肯定用过 React 路由器。它是一个功能丰富的路由库，那么它的内部是什么呢？

我们先来看需求。我们的主要特点是，我们不希望在跳转路线时刷新页面，而是只更新组件。然后浏览器里有一个`[popstate](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event)`事件可以…