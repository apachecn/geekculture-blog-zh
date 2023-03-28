# 惰性+悬念实现 React 组件惰性加载

> 原文：<https://medium.com/geekculture/lazy-suspense-implements-react-component-lazy-loading-a32b07af930f?source=collection_archive---------4----------------------->

![](img/cbd72742c03c3202b269b218347f38e7.png)

Photo by [sean Kong](https://unsplash.com/@seankkkkkkkkkkkkkk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

众所周知，随着我们前端项目的不断迭代，会包含越来越多的代码，项目打包后静态资源的体积也会不断扩大。这直接导致用户打开网站时页面加载时间不断延长，进而影响用户体验。尤其是当页面内容被无优先级加载时…