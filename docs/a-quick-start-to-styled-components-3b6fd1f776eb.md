# 样式组件快速入门

> 原文：<https://medium.com/geekculture/a-quick-start-to-styled-components-3b6fd1f776eb?source=collection_archive---------32----------------------->

## 将 CSS 添加到 React 应用程序的更好方法

![](img/beb16c50b78d5e4a2807c3aac1569624.png)

Photo by [Negative Space](https://www.pexels.com/@negativespace?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/grayscale-photo-of-computer-laptop-near-white-notebook-and-ceramic-mug-on-table-169573/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

无论您是全栈开发人员还是前端开发人员，您都有可能使用或至少研究过 React.js 作为您的前端框架(它实际上是一个库)。在[单页面应用的世界里](/@NeotericEU/single-page-application-vs-multiple-page-application-2591588efe58) React 已经赢得了许多人的心，因为它快速、简单、可伸缩。React 的易用性和可读性部分是因为它遵循单一数据流和组件层次原则。

然而，在 React 中设计组件的样式并不简单。你要么不得不在你的 [JSX](https://reactjs.org/docs/introducing-jsx.html) 中使用内嵌样式，要么不得不走传统路线，使用 CSS 并链接各种样式表。内联样式的问题是它的效率有点低，而且它肯定会使你的代码可读性差甚至笨拙。如果您选择使用 CSS 样式表，这很好，但是感觉不太“像 React ”,因为 React 思想的一部分是将所有相关的东西放在一起。必须通过类名来选择组件，然后通过样式表将它们连接到适当的样式，这种想法偏离了 React 试图解决的问题。

内联样式和 CSS 样式表共有的另一个问题是很难动态呈现样式。React 给了我们改变应用程序和动态呈现新事物的能力。它脱离了过去传统的枯燥的静态网站。然而，使用这两种造型方法没有简单的*方法。那么，解决方案是什么？*

*答案: ***样式组件！****

# *什么是样式组件？*

*React 社区已经创建了样式化的组件来满足我们在应用程序中的样式化需求。Styled-Components 是一个库，允许开发人员将 CSS 直接写入他们的 JavaScript 文件。是的，一开始可能会觉得奇怪，但是一旦你尝试了，我肯定你不会回头！*

*使用样式组件库，您可以创建定制的、可重用的 HTML 元素，作为 JavaScript 文件中的组件。这允许您直接定制您在应用程序中创建的每一个组件或元素，而不必经历处理 CSS 类和特殊性的麻烦。*

# *为什么应该使用样式化组件？*

*使用样式化组件有很多好处。当您合并样式化的组件时，您就有了一种更简化的方法来管理应用程序的各种样式。由于 CSS 直接发生在 JavaScript 文件中，所以一切都耦合在一起，并且更容易跟踪和调整每个 HTML 元素在屏幕上的呈现方式。这消除了有时遍历样式表以找到组件的 CSS 所在的确切位置的艰苦过程。*

*使用样式化组件的另一个主要原因是能够将*道具*传递给样式化组件，以便动态呈现 CSS。是的，现在你可以让 CSS 依赖于你的应用程序中不断变化的*状态*，这可以根据用户输入转换成不同的样式！能够将动态样式的道具传递到组件中符合使用 React 的基本工作流程。*

# *初始设置*

*假设您已经设置了 React 应用程序(否则您为什么要尝试使用样式化组件)，设置样式化组件既快速又简单。*

1.  *在 React 应用程序的目录中，在终端中运行以下命令之一:*

```
*npm install styled-components
yarn add styled-components*
```

*2.在您想要创建样式化组件的 JavaScript 字段中，包括以下内容:*

*`import styled from "styled-components"`*

*3.您现在可以开始使用样式化组件了！*

# *语法/如何使用样式化组件*

*为了在应用程序中使用样式化的组件，您已经使用`styled`方法创建了一个新的组件，然后您就可以像平常一样处理普通的 CSS 了。当使用`styled`方法时，你可以将它与任何你想要添加自定义样式的 HTML 元素配对，后面跟着保存你的 CSS 的反勾号。下面是一个代码示例，展示了我们如何实现基本样式的组件。*

# *结论*

*现在你已经合并了你的第一个样式组件，我相信你已经意识到它们是多么的简单和有用。它允许在 React 应用程序中轻松实现 CSS，并将每个组件的所有信息放在一个位置。这使得跟踪和编辑应用程序的不同风格变得更加容易，而不必遍历冗长的样式表或庞大的代码。*

*如果您有兴趣了解更多关于样式化组件的知识，请阅读本文的第 2 部分！*

*编码快乐！*

## *有用的资源*

*   *[https://styled-components.com/](https://styled-components.com/)*
*   *[https://github.com/styled-components/styled-components](https://github.com/styled-components/styled-components)*
*   *[https://www.youtube.com/watch?v=c5-Vex3ufFU](https://www.youtube.com/watch?v=c5-Vex3ufFU)*
*   *【https://www.youtube.com/watch?v=NMiEREulVLc 号*