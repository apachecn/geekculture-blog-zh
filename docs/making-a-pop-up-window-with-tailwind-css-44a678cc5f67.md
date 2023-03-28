# 用 CSS 制作一个弹出窗口

> 原文：<https://medium.com/geekculture/making-a-pop-up-window-with-tailwind-css-44a678cc5f67?source=collection_archive---------22----------------------->

![](img/4843a33c25c5bc5863e934ac2762e221.png)

Photo by [Joel Filipe](https://unsplash.com/@joelfilip?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/architecture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

最近，我开始为自己做一个名为[绿色事物](https://green-things.thomasstep.com/)的小项目，我想将大部分设计建立在一个页面上(至少在开始时是这样)。我的想法是有一个植物列表，用卡片表示，当我打开网站时，当我点击特定植物的卡片时，我希望出现一个弹出窗口，显示关于该植物的更多信息。这看起来很简单，但是当我去实现它的时候，我在思考 CSS 的一些概念时遇到了一点麻烦。在这篇文章中，我希望解释我是如何创建一个弹出窗口的，以及为什么它是这样工作的。

我这个项目的目标是在网站中使用尽可能少的 Javascript，使它快速而轻便。另一个目标是学习更多的 CSS，因为出于某种原因，我一生都在通过猜测和检查定位来避免学习网页布局是如何工作的。这是我更好地熟悉 web 开发中最重要的部分的机会。开门见山地说，下面是我用来创建弹出窗口的顺风类。这段代码还可以在它所在的 [GitHub repo 上获得更多的上下文信息](https://github.com/thomasstep/green-things/blob/main/components/plantCard.jsx)。

```
<div
  className={`overflow-auto \
              z-30 \
              h-5/6 \
              w-10/12 \
              mx-auto \
              top-20 \
              p-6 \
              border \
              rounded-xl \
              bg-white \
              text-left \
              fixed \
              ${visible ? 'visible' : 'invisible'}`}
>
```

我承认我仍然使用 React state 来帮助确定最后一个类，所以如果我想完全摆脱对 Javascript 依赖，我还需要做一些工作。我建议要么不使用`visible`的行，传递一个`visible`道具，要么保持一个`visible`状态以使其正确工作。不管怎样，我想指出几个类:`fixed`、`z-30`、`h-5/6`、`w-10/12`、`top-20`、`mx-auto`和`overflow-auto`。几乎所有这些类都被用来在屏幕上正确地定位和调整弹出窗口本身的大小，而`overflow-auto`更多地与内容本身相关。

我必须克服的第一个怪异之处是`fixed`。我不会说太多，但在我来`fixed`之前，我已经开始使用`flex`和`absolute`的组合。在我弃船开始使用`fixed`之前，我用`absolute`摆弄了太久定位`div`。通过在`div`上使用`fixed`，我或多或少地告诉`div`无论用户在窗口上滚动到哪里，都要保持它的位置。这是我从一开始就想要的，但是在我意识到我应该使用一个完全不同的 CSS 属性之前，不知何故我能够让`flex`和`absolute`有些工作。

在`z-30`的帮助下，`fixed` `div`优先于屏幕上的任何内容。一个网页上有 3 个轴，`z`轴控制哪些元素上下堆叠。所有元素自动以值为`0`的`z`开始它们的存在，因此通过将我的弹出窗口设置为值为`30`的`z`，我能够使它在页面上的其他元素之上弹出。

为了控制弹出窗口的边界，我使用了`h-5/6`和`w-10/12`，它们根据屏幕的大小分别定义了高度和宽度。我觉得这些尺寸非常适合我的内容，也让我的页面保持了响应性。值得注意的是，尺寸占据了页面的大部分，所以如果弹出窗口不太显眼，可能需要缩小尺寸。

接下来的两个类帮助在页面上正确定位`fixed` `div`。设置`top-20`帮助我避免了窗口在页面上被压得太低(尽管这可能更依赖于我在文档中放置 HTML 元素的方式)。`mx-auto`类的目的是保持窗口在水平轴上居中；然而，在摆弄它之后，我不太确定它是否 100%有必要实现我想要的布局。

终于来了`overflow-auto`。当我开始使用弹出窗口时，我没有太多的内容可以显示，但是在我添加了更多的文本和新元素后，我开始注意到所有内容的显示方式存在问题。在较小的屏幕上，内容从弹出窗口中溢出。这个问题比我想象的要容易解决。将`overflow-auto`添加到父`div`中，通过添加滚动条来访问更深层(现在是隐藏的)的内容，使其子代符合其父代的尺寸。这个解释可能有点混乱，但是一旦你玩了这个类，你就会明白我用它完成了什么。

尽管我只在这个项目上工作了几天，但我觉得我已经被介绍给了那些我已经远离自己很久的话题。我很兴奋地看到我在未来还能从顺风中学到什么。回到大楼！

*原载于 2021 年 5 月 12 日 https://thomasstep.com**的* [*。*](https://thomasstep.com/blog/making-a-pop-up-window-with-tailwind-css)