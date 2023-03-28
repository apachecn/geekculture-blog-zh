# 用动态类生成器编写更好的 SASS

> 原文：<https://medium.com/geekculture/writing-better-sass-with-dynamic-class-generators-e486a0413d0d?source=collection_archive---------20----------------------->

![](img/aa8c313650f1f3a117c7e42f2dbab8f9.png)

Photo by [Pixabay](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/abstract-architecture-black-and-white-boardwalk-262367/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在前端开发领域，我们可以使用大量的工具。似乎总是有新的闪亮的东西，似乎我们总是不得不更新我们的知识库。

虽然在 JavaScript 的世界里有很多变化，但是有几个常数。其中一个常量是样式化我们的代码。随着 SASS 的创建，我们作为前端开发人员有了一个非常强大的工具。我们应该理解这些功能，并尝试利用它来编写强大的、可维护的、最重要的是可读的代码。

如果你和我一样，那么你可能讨厌混乱的样式表。没有什么比打开样式表看到打破枯燥方法的代码更让我烦恼的了。为什么有 20 个类做同样的事情，除了一个数字或字母？我希望向你展示一种方法来防止这种事情再次发生！

在这个例子中，我将展示一些我认为我们都曾遇到过的东西。假设您被要求构建处理最小和最大宽度或高度的类，类的大小为 0 -> 2 像素。在老式的 css 文件中，我们必须做如下事情:

Really repetitive code

这个超级重复。这也是写的痛苦，维护的痛苦。如果有更好的方法呢？

Cleaner approach

这是一种更干净的方法。我们利用映射，然后从每个映射中获取值，最后执行一个循环来生成我们的类。我们传入几个变量来给我们的类名一个合适的结构，它为我们做了所有困难的工作。

理想情况下，我们不希望使用 2 个随机数作为我们的约束，所以我们理想情况下会创建第三个值的映射。我发现这个例子也展示了 scss 领域中的另一个工具。我们现在可以使用我们的类名，看起来像是`minimum-width-2`或`maximum-height-1`。

这个思考过程可以应用到我们样式表的任何区域。我发现这对于 flex 以及边距和填充之类的东西来说很好。通过学习重复的课程和使用更实用的文件，它将在未来为你省去一大堆麻烦。

我推荐在这里查看官方文件。如果你想看看我的其他帖子，可以在这里找到。我写所有前端特有的东西，所以我在[反应](https://jgrice01.medium.com/react-basics-lets-create-a-class-based-component-part-1-of-2-7249f7fac75e)、[打字](https://jgrice01.medium.com/typescript-understanding-the-basics-a2264759cd2d)、& [电子](https://jgrice01.medium.com/want-to-build-desktop-apps-using-js-say-hello-to-electron-4f862c3b4e38)上有帖子。

我希望这篇文章是令人愉快的。如果你有任何反馈，发表评论，让我知道我可以改进的地方。感谢您的阅读，祝您愉快！