# Jenkins & Khttp |使用 REST API 找出最后一次成功的构建

> 原文：<https://medium.com/geekculture/jenkins-khttp-use-rest-api-to-find-out-the-last-successful-build-6452cd2ff0d9?source=collection_archive---------6----------------------->

![](img/ac0a5853af890669a00d763785c06ba4.png)

Photo by [alevision.co](https://unsplash.com/@alevisionco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这篇文章是关于使用 [Jenksins](https://www.jenkins.io/doc/) REST API 来获取关于上一次成功构建的信息。Jenkins 没有官方的 Java/Kotlin API。这意味着，我们要么必须使用一些第三方 Github 开发者的 java API，要么仅仅使用 [Jenkins REST API](https://www.jenkins.io/doc/book/using/remote-access-api/) ，这是保证工作的。

# 问题是

我有一个 Jenkins 实例，其上运行着不同的作业。我需要另一个项目的最后一次成功构建的 git 散列。

我能怎么做呢？我可以转到 Jenkins，向下滚动到最后一个成功构建的分支，然后单击它。git 散列正在修订中，只需要从那里复制即可。如果你需要一天做 20 次，那会很伤脑筋。另一个使用案例是，您希望自动执行需要最后一次成功构建散列的操作，但您不能每次都手动输入。

# 解决方案

我说过，解决这个问题至少有两种方法。对我来说，解决方案的安全性是可靠的，并且有一个大的社区支持它，这真的很重要。我决定不使用非官方的 [Java API](https://github.com/cdancy/jenkins-rest) ，而是坚持使用 REST API 调用。如果 Java API 在阅读时获得了官方支持，请务必也查看一下。

现在让我们从 build.gradle 的配置开始。

将此依赖项添加到 build.gradle 文件中:

```
*implementation* ("khttp:khttp:0.1.0")
```

创建一个名为 Jenkins 的类，它有两个功能。

## 生成响应

1.  从 [khttp](https://khttp.readthedocs.io/en/latest/) 导入所有必要的依赖项。
2.  用您的 jenkins-server-address、job 和 branchName 创建一个简单的 get 请求。
3.  此 url 的最后一部分必须是*/lastsccessfulbuild。*
4.  使用 Jenkins 用户名和密码创建基本授权。

## 获取上一次成功的构建

1.  将“/”替换为“%252F”(因为这是 url 中的特殊字符)
2.  通过调用前面定义的私有方法来创建响应
3.  如果 statusCode 为 200，则从 HTML 中获取子字符串。(这个子串就是 git 散列。)
4.  如果 statusCode 不是 200，则返回一条错误消息

# 如何测试

用 Junit 创建一个新的 Testclass。

为了测试这个类的功能，我们需要模拟一个新的响应。我为此使用了[mock](https://mockk.io)，它基本上只是 Kotlin 的 [Mockito](https://site.mockito.org) 。因为我们只检查 statusCode 和响应的文本，所以我们只需要为它们定义行为。

*确保您已经为响应添加了正确的 khttp 导入。*

接下来，我们需要用 [spyk](https://mockk.io/#spy) 创建一个我们类的 spy，并定义我们私有方法的行为。因为这是一个私有方法，我们不能像以前那样使用它。我们需要这样称呼它:

```
jenkins["generateResponse"]("someJob", "someBranch")
```

然后，我们创建一个散列变量，它就是 getLastSuccessfulBuild 函数的输出。

最后一步是使用 assertThat 将哈希值与期望值进行比较。

## 测试错误消息

这实际上与之前的测试相同，不同之处在于您将 statusCode 更改为 200 以外的值。

现在，您需要用正确的作业和分支名称来断言错误消息，而不是断言散列。

# 反射

## 下次我还会这样做吗？

是的，有点。我认为我为这个项目使用了正确的组件。下一次我唯一想改变的是先多读一点 mockk 的文档。我对[嘲笑私有函数](https://mockk.io/#private-functions-mocking--dynamic-calls)有一些问题。一开始我甚至不知道这是可能的，但是它在[文档](https://mockk.io/#private-functions-mocking--dynamic-calls)中有很好的记录。

## 什么进展顺利？

对 REST API 的调用在开始时工作得非常好。除了那个私有函数之外，模拟也运行得很好。

## 有哪些需要改进的地方？

下次我会对模仿特殊方法做更多的研究，并阅读更多的文档。

我希望这篇文章能帮助你😺