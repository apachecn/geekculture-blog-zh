# React.js 如何工作的初学者指南

> 原文：<https://medium.com/geekculture/beginners-guide-on-how-react-js-works-22ee2733edd1?source=collection_archive---------36----------------------->

![](img/f3945fadc53c7c273d132b77e69116f0.png)

React 被称为 javascript 的用户界面库。这里要注意的一点是，react 是一个“库”，而不是 Angular 或 Amber 那样的框架。所以，首先我们来讨论一下框架和库的区别。

框架有助于轻松制作应用程序。此外，已经有许多智能设计存在。但问题是它不灵活，人们不能根据自己的需要定制它。如果一个人想用一小部分，他们必须用全部。

由于 react 是一个 javascript 库，所以它必须被导入到代码中。如果想使用 react，他们可以导入 react，如下所示:

fig 01

在 react 应用程序中，有一小部分是纯 react。他们中的大多数是许多其他东西的组合，例如，节点，JSX，巴别塔，网络包等。在这篇文章中，我们将对 JSX 做一个简要的讨论。但在理解这一点之前，必须对 DOM(文档对象模型)有一个清晰的认识。

DOM 基本上是用户在浏览器中看到的内容的表示。react 所做的是创建一个 DOM 的虚拟版本，称为“虚拟 DOM”它的作用是当程序第一次在浏览器中执行时创建一个虚拟 DOM。当程序中有一些变化时(例如，输入名字，勾选复选框)，它创建一个新的虚拟 DOM。然后，它使用一种叫做“差分算法”的算法来比较两个虚拟 DOM。如果两个 DOM 之间有任何差异，则更新第一个 DOM。React 非常高效地完成了所有这些工作，并且花费的时间非常少。

JSX 允许用 javascript 编写 HTML 代码，反之亦然。让我们看一个简单的例子，一段 react 代码，没有 JSX，有 JSX。

fig 02: React code without JSX

fig 03: React code with JSX

很明显，用 JSX 编写的 react 代码比没有 JSX 的代码更加清晰易懂。图 03 所示的代码实际上在浏览器中编译成图 02 的代码。脸书的开发商想出了 JSX 的主意。这里还有一点很重要，JSX 不是 HTML。它实际上是一种妥协。我们没有写硬 react 代码，而是选择写一些类似 HTML 的代码。浏览器无法理解 JSX。

如果我们试图在 JSX 内部编写元素，浏览器将抛出一个错误。正确的代码将是