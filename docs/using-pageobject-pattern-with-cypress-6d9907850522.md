# 对 Cypress 使用 PageObject 模式

> 原文：<https://medium.com/geekculture/using-pageobject-pattern-with-cypress-6d9907850522?source=collection_archive---------10----------------------->

![](img/b19476e0295c17835a49caee8b75b8a7.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

本文展示了如何使用带有 [Cypress](https://www.cypress.io/) 和 [cypress 选择器](https://anton-kravchenko.github.io/cypress-selectors/)的 [PageObject](https://martinfowler.com/bliki/PageObject.html) 模式。

页面对象模型(POM)是一种设计模式，旨在将网页表示为一个类，将该页面的元素描述为类字段，将该页面的行为描述为类方法。这类套件既是页面上 web 元素的存储库，也是与这些元素进行交互的接口。这种方法有助于在应用程序的布局上创建一个抽象，并封装了特定于应用程序的 API 背后的查询和交互机制。

页面对象模型模式有许多优点:

*   它为与 UI 的交互引入了方便的抽象
*   它将页面的 UI 结构及其行为的细节封装在一个类中
*   它支持代码重用，因为任何页面对象都可以在许多测试用例中使用
*   它简化了测试维护，因为很容易找到需要更新的页面和元素
*   由于 POM 将查询元素的复杂性隐藏在简单的高级 API 之后，它使得测试用例更具可读性，更容易理解

POM 是一种被广泛采用的模式，事实上是 E2E 测试的标准方法。尽管 Selenium web 测试框架创造了该模式的名称，但是 POM 模式可以用于其他测试框架。特别是，我发现 POM 模式在用 Cypress 编写的测试中很有帮助，所以让我们来看看如何结合使用这两种模式。

作为一个测试中的应用程序，我将使用名为 [conduit](https://cirosantilli-realworld-next.herokuapp.com/) 的[现实应用](https://github.com/gothinkster/realworld)的实现之一。这是 Medium.com 的翻版，你可以在这里创建账户、发表新文章、查看提要等等。

下面是我们将要测试的三个流程:

1.  登录流程—用户登录到应用程序
2.  发布新文章流—用户登录应用程序并发布文章
3.  列表发布流—用户登录到应用程序并打开文章列表

让我们从定义页面对象开始。测试用例覆盖了五个不同的页面，所以我们需要声明五个类:`LoginPage`、`HomePage`、`NewArticlePage`、`ArticlePage`和`ProfilePage`。

让我们从`LoginPage`开始:

第一个类使用来自 [cypress-selectors](https://anton-kravchenko.github.io/cypress-selectors/) 库的 decorators 将选择器声明为静态字段。属性`type`的值将选择`emailInput`和`passwordInput`元素。`signInButton`将指向页面上带有文本“登录”的第二个元素

第二个类是登录页面的页面对象。它实现了该页面上唯一可以做的事情——登录。选择器和页面行为如此分离的原因是，这样可以更容易地重用选择器，而不必在不必要时创建页面对象的实例。您的应用程序可能具有“全局”元素，这些元素总是出现在屏幕上，代表应用程序的状态(例如，如果您已登录，则显示您的信息的标题，否则显示登录按钮)。在这种情况下，最好只描述一次这些元素的控件，并在任何地方使用它们，而不是在许多类中声明相同的选择器。

仔细看看`LoginPageSelectors`类中的选择器是如何被使用的。值得指出的是，即使选择器已经被静态声明并被引用使用，实际的元素查找发生在下一个命令之前**。例如，这里的`LoginPageSelectors.emailInput.type(email)` Cypress 将在**执行`type`方法之前搜索`emailInput`元素**。**

每次使用选择器时，都会有一点小花招，但是唯一要知道的是选择器并不包含对任何特定 DOM 元素的引用。相反，它查询一个元素，并在每次调用它时返回一个对`Cypress.Chainable`的引用(与`cy.get`命令的方式相同)。因此下面两行代码是等效的:

```
LoginPageSelectors.emailInput.type(email);
cy.get('[type="email"]').type(email);
```

接下来是`HomePage`:

这里的方法是相同的:一个类具有静态声明性选择器，第二个类表示可以与页面进行的所有交互。在这个例子中，`HomePage`是一个“路由器”，所以我们有三个方法带您到不同的页面，并返回这些页面的页面对象的实例。

下一节课是`NewArticlePage`:

该页面允许创建带有标题、描述、内容、标签的新文章，并发布它。请注意，大多数方法都返回`this`。当您在一个页面上运行几个后续操作时，它允许方便地链接测试。

最后但同样重要的是，我们需要为`ArticlePage`和`ProfilePage`声明选择器:

现在我们已经准备好开始实施测试了。让我们从登录流程开始。该测试将打开登录页面，输入凭证，尝试登录到应用程序，并验证登录是否成功。

就这样——漂亮又简单。真实世界的测试用例要重得多，但是这里的想法是 POM 有助于在一个简单的接口后面隐藏许多实现细节。因此，当您为元素声明了选择器并描述了页面的行为之后，就可以用它来定义简洁易读的测试了。下面是在没有页面对象和选择器的情况下实现的相同测试:

测试充斥着 UI 细节，很难判断它实际上做了什么。与不使用任何抽象并通过硬编码的选择器查询元素相比，使用高级域概念(如我们的登录流程)的测试更容易理解和使用。

我们要测试的下一个流是不是发布文章流:

这个测试更强大一些——我们登录，获取对新文章页面对象的引用，用数据填充新文章表单，发布它，最后断言文章已经成功发布。即使这个测试比前一个更复杂，它仍然是可读和容易理解的。

最后，最后一个测试涵盖了个人资料页面:

在这里，我们登录到应用程序，打开配置文件页面，并验证文章提要是否包含最近发布的文章。

从例子中可以看出，POM 在很多方面让生活变得更简单。在花一些时间创建页面对象之后，您会获得许多好处，比如高可读性和较低的维护成本。

值得指出的是，POM 模式被认为是 Cypress 中的反模式。根据的文章[所述，页面对象很难维护，并且会使测试变慢，因为它们强制通过 UI 构建所需的应用状态，而不是采取快捷方式(比如使用](https://www.cypress.io/blog/2019/01/03/stop-using-page-objects-and-start-using-app-actions/)[动作](https://www.cypress.io/blog/2019/01/03/stop-using-page-objects-and-start-using-app-actions/#application-actions)修改应用的状态)。这些观点在某些情况下是有效的；然而，我发现 POM 很有帮助，使用它的好处超过了将其引入项目的成本。

在 [Github](https://github.com/anton-kravchenko/cypress-page-object-example) 上可以找到包含完整示例的存储库。

感谢您阅读文章，祝您编码愉快！