# TypeScript 是 JavaScript 的未来吗？

> 原文：<https://medium.com/geekculture/is-typescript-the-future-of-javascript-f121aa875464?source=collection_archive---------16----------------------->

探索 TypeScript 及其流行的原因。

![](img/22566791fe7b760f16653013b87401e2.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

作为一名刚毕业的软件工程专业学生。我看到越来越多的公司在他们的工作列表中寻找打字经验，今天我将简单解释一下什么是打字，以及为什么员工在寻找打字开发人员。

# 什么是 TypeScript？

TypeScript 是微软在 2012 年设计的一种编程语言，它源于 JavaScript 开发大型应用程序的缺点。TS(TypeScript)基本上是 Javascript 的超集，它在编程中添加了可选的静态类型来指定对象。TS 使用编译器将 TS 代码编译成 JS(JavaScript)。由于增加的功能和效率，TypeScript 自 2012 年首次亮相以来，在编程社区中出现了爆炸式增长。根据 2020 年 Github 语言数据，TypeScript 的受欢迎程度已经超过了 C++/C#和 Ruby 等语言，并且需求仍在与日俱增。

# 为什么使用 TypeScript？

使用 TypeScript 有许多原因，但这里列出了我使用 TypeScript 的利弊

## 赞成的意见

*   ***TypeScript 编译器-*** 这大概是我最喜欢用 TS 的原因。使用 TypeScript，您可以使用 TSC(TypeScript 编译器)来选择 ECMAScript 的目标版本，以将 TS 代码翻译到其中。如果你知道你的目标客户群没有现代的浏览器，你可以简单地设置你的 TSC 来编译你的代码到可以在旧的浏览器上运行的旧版本的 JS 中，这是非常有用的。
*   ***静态类型-*** Javascript 是动态类型的，直到代码运行时才知道变量是什么类型。TypeScript 添加了为变量指定类型的选项，这将有助于在 IDE 级别捕获大量基于类型的错误，而不是运行时错误，例如将字符串“1”而不是数字 1 添加到变量中。
*   ***IDE 支持-*** 它提供了大量与您的代码相关的内置信息由于 TypeScript 是由微软开发的，所以它得到了 Visual Studio & VS code 以及 Atom 和 Sublime editor 等各种 IDE 的广泛支持。

*   *****跨平台***——TypeScript 编译器可以安装在 Windows、MacOS、Linux 等流行的操作系统上。**

## **骗局**

*   *****额外代码-*** 你将不得不花更多的时间写代码，如果你不得不在一个大项目中指定一堆变量的打字。在我看来，这个缺点也是一种优点，你今天可能不得不花一点额外的时间来写代码，但是防止未来小错误的价值是一个很好的权衡。**
*   *****学习曲线-*** 由于 TypeScript 是 JavaScript 的一个超级集合，所以学习曲线会很长，因为为了使用 TS，你最终必须学习 JS。**

*   *******项目转换-*** 由于严格类型化的复杂性，将较大的旧 JavaScript 应用程序转换为现代类型化应用程序可能会很麻烦且耗时。****

# ****如何安装 TypeScript****

****您可以简单地用您的包管理器(如 yarn 或 npm)安装 TypeScript****

```
**npm install -g typescript**
```

****该命令的作用是将 TypeScript 编译器安装到本地环境中。安装完成后，您可以创建。ts 文件，并用 tsc 命令编译它们。****

```
**//Exampletsc app.ts**
```

****一旦编译器完成，您将看到一个. js 文件以及您的。刚刚编译的 ts 文件。****

******学习基本打字稿的资源******

*   ****[打字稿:文档—新程序员打字稿](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html) [学习](https://www.codecademy.com/learn/learn-typescript)****
*   ****[打字稿|代码集](https://www.codecademy.com/learn/learn-typescript)****
*   ****[5 分钟学会打字——初学者教程](https://www.freecodecamp.org/news/learn-typescript-in-5-minutes-13eda868daeb/)****

# ****结论****

****TypeScript 的官方口号是**“可伸缩的 JavaScript”**，我发现这非常适合这种编程语言。因为我自己是一名 JavaScript 开发人员，所以我个人很喜欢切换到 TypeScript，以及它给传统 JavaScript 带来的新特性和新功能。****

****我目前正在用 TypeScript 进行一个个人的大规模项目，我将在未来提供关于如何使用 TypeScript 的博客，敬请关注！****

****感谢您的阅读，如果您喜欢，请留下评论，并关注更多编码内容。****