# 现代 Web 应用程序是如何开发的

> 原文：<https://medium.com/geekculture/how-modern-web-applications-are-made-today-514ff5fc8506?source=collection_archive---------10----------------------->

## 构建现代 web 应用程序需要哪些框架和技术

Web 开发是一个高速发展的领域，每天都有新的技术和改进。我们过去制作可以在台式电脑上浏览的 web 应用程序的日子已经一去不复返了。如今，随着手机、平板电脑、笔记本电脑等不同屏幕尺寸设备的出现，我们作为开发人员需要让我们的网站对所有设备都有响应能力。

今天的 Web 开发已经变得比 HTML、CSS 和普通的 JS 更加复杂。有了 React、Angular 这样的框架，我们可以制作高度可伸缩的 web 应用程序，并增加前端的复杂性。

在这篇文章中，我将谈论今天的 web 开发是如何成为一项极具挑战性的工作，以及现代技术如何改变了今天的 Web 应用程序的制作方式。

![](img/1c1058a072f0776c232d0b7a13c3435e.png)

Photo by [Domenico Loia](https://unsplash.com/@domenicoloia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 客户机-服务器模型

回到 5 年前，那时只有一台客户机和一台服务器。客户端(即用户的浏览器)向服务器发送一个请求，服务器用一个包含 CSS 和 JS 的 HTML 文件作为响应，该文件被加载到浏览器上。每当用户单击按钮或进行更改时，浏览器发送一个 HTTP 请求，服务器处理请求，发送 HTML 响应并重新加载页面。这是客户机-服务器模型，前端只是普通的 HTML、CSS 和 JS，后端服务器做繁重的工作。

## 网站和网络应用的区别

许多人交替使用这两个术语。如果 5 年前人们会说网站和网络应用是一样的，这可能会被接受，但现在网络应用已经变得更加强大。

根据定义:一个网站是一组互连的网页托管在一个网络服务器有一个单一的域名。

web 应用程序是一种软件或程序，可以驻留在单个服务器或多个服务器上，并可以加载到浏览器上。React 和 Angular 等框架在客户端浏览器中预加载 javascript 和 HTML 文件作为一个包，并使用 API 请求执行任务。

## 单页应用程序

你每天打开的大部分网站都是单页应用，如脸书、谷歌地图、Gmail、Github、Medium 等。

> **单页应用是指单页网站吗？**

答案是否定的。单页应用程序是一种在使用过程中不需要重新加载页面的应用程序，它可以在浏览器中工作。应用程序被划分为不同的组件，这些组件可以重用。当发生更改时，只重新呈现特定的组件，而不是重新加载整个页面。此外，在从一个部分导航到另一个部分的过程中，组件被隐藏并显示另一个组件，而不是重新加载页面和路由。

单页应用程序的主要优势是速度。HTML、CSS 和 Javascript 文件作为一个包预加载到浏览器上，因此，应用程序不需要重新加载，对用户的响应非常快。

![](img/f1630eab6e6210df54cbebeaa48c0a42.png)

Photo by [Taras Shypka](https://unsplash.com/@bugsster?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## REST API

我们已经看到了客户机-服务器模型，其中服务器做繁重的工作。但是有了 React 和 Angular 这样的框架，前端现在也可以完成以前不可能完成的高度复杂的任务。

因此，服务器所要做的就是向这些前端应用程序提供数据，它们现在处理复杂的任务，服务器可以连接到数据库并传输数据。今天大多数 API 使用 JSON——JavaScript 对象符号轻量级数据交换格式来共享数据

服务器使用端点向前端响应 JSON 请求。

## 移动网络和渐进式网络应用

如今，70%的用户使用移动浏览器浏览网站。因此，我们必须创建 web 应用程序，同时牢记移动体验。事实上，许多开发人员在设计网站时都遵循移动优先的设计模式。在媒体查询和 Bootstrap 等框架的帮助下，响应式网站成为可能

渐进式 web 应用程序结合了 web 和移动应用程序的优点。渐进式 web 应用程序是一种通过 web 交付的应用程序软件，使用常见的 web 技术(包括 HTML、CSS 和 JavaScript)构建。它可以在网络和移动平台上运行，包括移动设备的所有功能，这是传统网络应用程序所不能做到的。您可以使用 React、Angular 或 Ionic 等框架创建渐进式 Web 应用程序。

谢谢你一直读到最后，希望这篇文章对你有所帮助。请关注我，获取更多此类文章！！！

# 参考资料:

[](https://huspi.com/blog-open/definitive-guide-to-spa-why-do-we-need-single-page-applications) [## 软件开发和 IT 咨询

### 我们习惯了上网，习惯了用手拿着手机，以至于我们甚至没有停下来想一想…

huspi.com](https://huspi.com/blog-open/definitive-guide-to-spa-why-do-we-need-single-page-applications) [](https://web.dev/progressive-web-apps/) [## 网络开发

### 在这一集中，你将了解到什么使一个进步的网络应用程序特别，它们如何影响你的业务，以及如何…

网络开发](https://web.dev/progressive-web-apps/)