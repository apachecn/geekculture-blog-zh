# 用 HTML、Bootstrap 和 JavaScript 创建一个静态网站

> 原文：<https://medium.com/geekculture/creating-a-static-website-with-html-bootstrap-and-javascript-fcbdf3254bcb?source=collection_archive---------0----------------------->

## Web 开发教程

## 包含示例代码的分步指南，帮助用户入门

![](img/cdbc6aa5ff8a476c6aaba6c52227dd2f.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

随着用户寻找建立在线品牌的方法，他们转向个人/专业网站来展示他们的项目和工作经验[1]。

据《福布斯》报道，以下是用户需要建立自己网站的三个原因[2]:

1.  网站让用户可以控制自己的形象，用户现在可以决定他们的教育、作品集或个人生活经历的哪一部分值得强调。
2.  **现在建立一个覆盖范围可以帮助以后的用户**——网站越广泛，流量就越高。这意味着用户可以有更多的联系、机会等等。
3.  **网站让用户在竞争中脱颖而出**——虽然网站可能类似于简历，但它们有潜力变得更加广泛和吸引人，这可以帮助用户将自己与他人区分开来。

在本教程中，我将介绍用户创建自定义静态网站所需的基本元素！

# 基本组件

![](img/59fdc25e2edae41529ffc46172825fa6.png)

Photo by [Ross Sneddon](https://unsplash.com/@rosssneddon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

创建个人网站最简单的方法是使用 HTML 进行基本的文本布局，使用 Bootstrap 进行整体设计，使用 JavaScript 实现任何需要的附加功能！

## 超文本标记语言

超文本标记语言(HTML)是网页的标准标记语言，其元素被认为是任何网站的构建模块[3]。

首先，用户需要创建一个基本的 HTML 模板，他们可以向其中添加引导程序和 JavaScript 代码。

如下所示，基本模板将包括“html”、“head”、“body”和“meta”标签，这些标签将包含网站的大部分源代码:

Basic HTML Outline

## 引导程序

为了指定 HTML 元素的显示风格，使网站更具响应性，更优雅，用户可以使用层叠样式表——CSS[4]。

用户可以简化这个过程，而不是添加单独的 CSS 文件来概述网站设计，而是使用一个流行的 CSS 框架 Bootstrap 来利用预定义的 CSS 设计模式，这些模式在桌面、平板电脑和移动设备上运行良好[5]。

为了利用 Bootstrap，用户需要在其 HTML 文件中添加以下 CSS 依赖项:

Include Bootstrap CSS Dependency

## Java Script 语言

JavaScript 是一种 web 编程语言，在计算、操作和验证数据时可以改变 HTML 和 CSS 元素[6]。

除了 Bootstrap CSS 依赖项，用户还需要添加必要的 JavaScript 依赖项，以便 Bootstrap 可以正确呈现网页。

如下所示，用户还可以添加定制的 JavaScript 代码来实现他们网站上的任何附加功能。在我的例子中,“smooth_scrolling.js”文件会在每次点击链接时降低页面的滚动速度:

Include JavaScript Dependencies

# 建立实际的网站

![](img/6a5d64866d48330f3b7ea4ca626389f1.png)

Photo by [Dan Burton](https://unsplash.com/@single_lens_reflex?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

现在所有的依赖项都包括在内了，真正的乐趣可以开始了！

虽然有许多不同的方法来建立一个网站，用户需要认识到，对于各种不同的用例，有些布局比其他的更有效！

例如，当决定如何最好地设计他们的网站时，用户需要关注可用性和美学等因素。

对我来说，我希望有以下几个部分:

1.  顶部的导航栏可快速链接到我的网页中的不同部分。
2.  图片转盘包括一些我的成就的照片。
3.  投资组合部分，我可以包括我在其他平台上的作品的链接。
4.  有兴趣联系我的人的联系方式。

因此，我最终使用了各种不同的“div”、“list”和“button”HTML 元素来指定我的网站的主要内容，并重点关注使整个网站更具响应性所需的引导程序。我还添加了一些定制的 JavaScript 代码，使滚动更加用户友好。

包含这四个部分的源代码的 GitHub repo，包括教程开始时使用的代码片段，可以在这里找到:[https://github.com/deeptaanshu/personal-website](https://github.com/deeptaanshu/personal-website)

该网站托管后，外观如下:

[](https://www.deeptaanshu.com/) [## 迪普塔安舒·库马尔

### 欢迎来到我的网站！请看看我的工作，并随时与我联系！

www.deeptaanshu.com](https://www.deeptaanshu.com/) 

# 结论

![](img/d896ae385f3ada4fd4e08a0538211487.png)

Photo by [Giorgio Trovato](https://unsplash.com/@giorgiotrovato?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

恭喜你，你现在有一个工作网站了！！！

总的来说，有很多方法可以创建静态网站，但这里有三个组件，用户可以结合起来快速轻松地开发网站:

1.  **HTML** —用户可以使用 HTML 标签指定网站中包含的元素。
2.  **Bootstrap**——用户可以利用预定义的 CSS 设计模式构建更优雅的网站，比如那些在 [Startboostrap](https://startbootstrap.com/previews/) 找到的网站。
3.  **JavaScript** —用户可以添加定制的 JavaScript 来实现网站特定的功能，例如当点击网站上的链接时平滑滚动。

一旦定义了这些组件，用户就可以专注于使用不同的设计主题和创建不同的布局来适应他们特定的用例。

成功创建网站后，下一步是将它放在网上，这样其他人也可以看到它。

在下一篇教程中，我将回顾我是如何在 AWS S3 上托管本教程使用的网站[的，这样我就可以在云上实现一个负担得起且可扩展的托管解决方案。](https://www.deeptaanshu.com)

敬请期待！！！

# 参考

[1]顶级个人网站示例:[https://collegeinfogeek.com/personal-website-examples/](https://collegeinfogeek.com/personal-website-examples/)

【2】为什么要创建个人网站:[https://www . Forbes . com/sites/Laurence Bradford/2016/09/27/3-reasons-Why-you-need-a-website/？sh=1fcd5de32460](https://www.forbes.com/sites/laurencebradford/2016/09/27/3-reasons-why-you-need-a-website/?sh=1fcd5de32460)

【3】什么是 HTML:【https://www.w3schools.com/whatis/whatis_html.asp】T4

【4】什么是 CSS:[https://www.w3schools.com/css/css_intro.asp](https://www.w3schools.com/css/css_intro.asp)

【5】什么是自举:[https://www.w3schools.com/whatis/whatis_bootstrap.asp](https://www.w3schools.com/whatis/whatis_bootstrap.asp)

[6]什么是 JavaScript:[https://www.w3schools.com/whatis/whatis_js.asp](https://www.w3schools.com/whatis/whatis_js.asp)

【7】创建网站需要考虑的事情:[https://www . mrnwebdesigns . com/designing-a-website-here-are-7-important-factors-to-consider/](https://www.mrnwebdesigns.com/designing-a-website-here-are-7-important-factors-to-consider/)