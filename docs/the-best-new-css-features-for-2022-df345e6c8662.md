# 2022 年最好的新 CSS 特性

> 原文：<https://medium.com/geekculture/the-best-new-css-features-for-2022-df345e6c8662?source=collection_archive---------0----------------------->

## 这里有七个你不想错过的级联样式表改进，从鲜为人知的滚动捕捉功能到令人惊叹的新调色板。

![](img/73b9d30db7dba3671fa105f48a1a94ac.png)

自 1996 年引入以来，级联样式表(CSS)已经发展成为 web 开发堆栈的重要组件。类似于其他现存的语言，CSS 不断增加新的功能来响应现实世界中的使用模式。即使是忠诚的开发者也会发现，由于快速的发展，忽略新的特性是很容易的。这里是一些最有益的 CSS 新功能和即将推出的功能。

# 子网格

自从 CSS 诞生以来，开发人员就一直抨击它有许多可怕的缺陷。一些常规操作，比如在 CSS 中居中内容，需要非常复杂的变通方法和欺骗。在 CSS 网格布局模块介入填补空白之前，建立一个可行的网格布局是另一个重大问题。网格布局用`display: grid`声明来表示，它有点类似于 [Flexbox](https://www.w3schools.com/css/css3_flexbox.asp) ，它允许你定义矩形布局，但也可以在二维空间控制网格。研究表明[我们接触过 CSS 的大多数开发人员都知道网格布局](https://2021.stateofcss.com/en-US/features/layout)，我们中的许多人都在使用它。

网格布局模块现在有了一个全新的非常有用的特性，叫做子网格。使用子网格功能，您可以创建一个模仿父网格设计的子网格。相反，当一个网格显示嵌套在另一个网格中时，子网格决定自己的大小和间距。子网格将父网格的布局应用于自身，尽管它保留了在必要时覆盖某些元素的能力。Subgrid 目前只在 Firefox 版本 71 和更高版本中可用，尽管它计划包含在 Google Chrome、Microsoft Edge 和 Safari WebKit 中。子网格将是未来布局的一个重要方面。

# 强调色

尽管经常被使用，一些显示组件在历史上具有挑战性的风格。例如，复选框和单选按钮经常被一个定制的小部件替代，这个小部件模仿它们的功能，同时隐藏浏览器的实现。您可以使用新的 CSS 强调颜色选项来定位这些组件。

例如，您可以对页面上的所有单选按钮应用洋红色，如清单 1 所示

## 清单 1。在 CSS 中控制单选按钮的颜色

```
**<style>**
input[type="radio"] {
    accent-color: magenta;
}
**</style>****<form** action="/foo.bar"**>**
  **<p>**Select your favorite outdoor adventure type**</p>**
  **<input** type="radio" id="mountain" name="type" value="mountain"**>**
  **<label** for="mountain"**>**Mountain**</label><br>**
  **<input** type="radio" id="ocean" name="type" value="ocean"**>**
  **<label** for="ocean"**>**Ocean**</label><br>**
  **<input** type="radio" id="desert" name="type" value="desert"**>**
  **<label** for="desert"**>**Desert**</label>**
**</form>**
```

# 滚动捕捉

CSS 为管理 web 浏览器的滚动捕捉行为提供了一个有用的属性集合。有些功能已经提供了一段时间，其他功能现在才提供给更现代的浏览器版本。

有趣的是，超过三分之一的 CSS 用户[仍然不知道滚动快照](https://2021.stateofcss.com/en-US/features/interactions/#scroll_snap)。

`scroll-snap-* properties`命令为您提供了多种方式来微调滚动位置在容器上的工作方式。开发人员获得更高的精确度，最终用户获得更流畅、更可控的用户体验。

清单 2 给出了一个控制`div`上滚动捕捉的小例子

## 清单 2。简单滚动捕捉示例

```
**<style>**
  .scroll-container,
  .scroll-area {
    max-width: 850px;
    height: 300px;
    font-size: 60px;
  } .scroll-container {
    overflow: **auto**;
    scroll-snap-type: y mandatory;
    height: 500px;
  } .scroll-area {
    scroll-snap-align: start;
  } .scroll-container,
  .scroll-area {
    margin: 0 **auto**;
  } .scroll-area {
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
  } .scroll-area:nth-of-type(1) {  background: **IndianRed**; }
  .scroll-area:nth-of-type(2) {  background: **Moccasin**; }
  .scroll-area:nth-of-type(3) {  background: thistle; }
  .scroll-area:nth-of-type(4) {  background: seagreen; }
**</style>****<div** class="scroll-container"**>**
	**<div** class="scroll-area"**>**1**</div>**
	**<div** class="scroll-area"**>**2**</div>**
	**<div** class="scroll-area"**>**3**</div>**
	**<div** class="scroll-area"**>**4**</div>**
**</div>**
```

无论您在清单 3 中的什么地方放开滚动，y 滚动位置总是前进到子元素。这是因为 scroll-snap-type 属性被设置为 scroll 容器上的 y required 以及子组件上的 scroll-snap-align: start 声明。

此外，此行为是可修改的。

例如，滚动捕捉类型属性可被设置为 y 接近度。它按预期运行，并且只在滚动接近元素时才捕捉。

此外，您可以通过设置关联的 overscroll-behavior 参数来控制嵌套滚动容器的操作方式。

# CSS 逻辑属性

每当您希望在左边和右边，或者底部和顶部建立容器边框时，都必须写出 border-left 和 border-right，或者 border-top 和 border-bottom 属性，这很烦人。问题是没有办法使用快捷方式属性而不影响您不想更改的边界。边距和填充是遭受这种不适的组件的例子。

得益于 CSS 逻辑属性模块，inline 和 block 关键字可用于对项目进行抽象引用。用 inline 来讲左右；用 block 来说顶部和底部。例如，要向 div 的左右两侧添加边框，可以使用清单 3 中的代码。

## 清单 3。逻辑内联的左右填充

```
div {
  border-**inline**: 10px dashed seagreen;
}
```

inline 和 block 逻辑关键字也可以在各种其他属性中找到，但它们是边框的特别有用的快捷方式。

开发人员经常使用这些快捷键来解决文本方向和书写模式问题。在这些情况下，您可以通过使用类似 padding-inline-end 的属性，独立于活动文本方向来定位尾部填充。本质上，内联和块的抽象允许创建可以在许多上下文中使用的通用样式。有关更详细的处理，请参见 CSS 逻辑属性和值。

# 容器查询

CSS 还没有完全稳定容器查询，但是他们会的。它们将对我们如何看待响应式设计产生重大影响。基本概念是，除了视口和媒体之外，您还可以根据父容器的大小来建立断点。

尽管语法没有完全指定，清单 4 可能是一个例子。

## 清单 4。@容器

```
@container (max-width: 650px){ … }
```

想想看，根据 UI 嵌套层中出现的各种容器的尺寸来调整布局会有多有效。毫无疑问，界面创新的浪潮将从这一重大变化中产生。

# 三种新的调色板

从一开始，CSS 设计者就采用 RGB、HEX 和命名颜色来增强和美化他们的设备显示。HSL 风格的色彩宣言最近才推出。CSS 标准现在增加了三种表示颜色的方式:`hwb`、`lab`和`lch`。

HWB 是色调、白色和黑色的缩写。你选择一种色调，添加白色和黑色，做出一个聪明的加法，以其人类可读性而闻名。它兼容最新版本的 Chrome、Firefox 和 Safari。奇怪的是，微软 Edge 功能参考中并没有提到这一点。要了解更多关于 HWB 的信息，请看 hwb() —人类的颜色符号？它与 alpha 通道兼容，很像 RGB 和 HWL。

首字母缩写词 LCH，代表亮度、色度和色调，以拓宽调色板而闻名。什么，为什么，如何在 CSS 中使用 LCH 颜色？是一个很好的总结，包含了对 CSS 中颜色理论的深刻讨论。LCH 目前仅受 Safari 支持。新色彩空间中理论上最复杂的是 LAB，它是根据 CIE LAB 色彩理论创建的。这是一个大胆的主张，lab 颜色描述符包括了人类能够感知的所有颜色。现在只有 Safari 和它兼容，很像 LCH。Mozilla CSS 文档中有关于 LAB for CSS 的更多信息。