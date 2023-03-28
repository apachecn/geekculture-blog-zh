# 使用顺风 CSS 将 Div 居中

> 原文：<https://medium.com/geekculture/centering-a-div-with-tailwind-css-f28d70e23cec?source=collection_archive---------20----------------------->

![](img/f3fb84e17650d582eb92a77a911e90a1.png)

Photo by [Jannic Böhme](https://unsplash.com/@jannic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/symmetrical?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

不知何故，以一个`div`为中心似乎仍然困扰着人们。我最近大部分时间都在使用 Tailwind CSS，我想快速分享一下我是如何用 Tailwind 将 div 居中的。我将首先展示例子，然后描述发生了什么。

```
<div class="flex flex-row min-h-screen justify-center items-center">
  I am centered
</div>
```

同样的类与`flex-row`或`flex-col`一起工作，它们分别水平或垂直设置 flexbox 的主轴。用`min-h-screen`设置高度只是占据整个屏幕视图的一种简单方法。最后两节课是我需要学习少量 CSS 的地方。

在我最终研究它们的影响之前，我花了太长时间来研究合理性和一致性。`justify-content`是指内容应该如何沿主轴定位，而`align-content`是指内容应该如何沿横轴定位。[调整内容](https://tailwindcss.com/docs/justify-content)和[用 Tailwind 对齐内容](https://tailwindcss.com/docs/align-content)是它们的类和实际 CSS 之间简单的一对一映射，所以一旦我理解了 CSS 是如何工作的，我就理解了实用程序是如何工作的。

*原载于 2021 年 7 月 23 日 https://thomasstep.com*[](https://thomasstep.com/blog/centering-a-div-with-tailwind-css)**。**