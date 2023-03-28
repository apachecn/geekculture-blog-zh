# 用 Goyave 框架进行 SQL 模拟测试

> 原文：<https://medium.com/geekculture/sql-mock-tests-with-the-goyave-framework-f3d598666b7e?source=collection_archive---------10----------------------->

![](img/75c3c5b9ac688bf66fcb38be06980f73.png)

Credits: [**panumas nikhomkhai**](https://www.pexels.com/@cookiecutter?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)on Pexels

[**Goyave**](https://github.com/go-goyave/goyave) 是一个 **Golang REST API 框架**，它尽力帮助你高效地开发应用。它的特性允许您关注真正重要的东西:应用程序的业务逻辑。

我们开门见山:今天我们就来说说**测试**。尽管框架已经提供了很好的测试工具，但是用真实的数据库进行测试并不总是可行的。当你写单元测试的时候，为一个简单而简短的函数建立一个数据库是非常麻烦的。这就是为什么在测试中模仿数据库非常流行的原因。但是你会怎么做呢？

实际上，在使用这个框架时，您并不直接管理 DB 连接。谢天谢地，它的设计没有牺牲灵活性。在最新发布的 v4.1.0 中，我们现在可以用一个 mock 手动替换数据库连接。让我们使用流行的 [go-sqlmock](https://github.com/DATA-DOG/go-sqlmock) 。

我们想要测试的代码非常简单:一个用户模型存储库和一个通过 ID 获取用户的函数。

让我们从创建 Goyave 测试套件的扩展开始。这个扩展将创建一个模拟数据库，并使它可用于套件中的所有测试。在这个例子中，我们使用 PostgreSQL。所以我们需要为这个引擎导入 GORM 驱动程序，从它创建一个**拨号器**，然后在 Goyave 中将它设置为活动连接。

得益于**组合，**我们可以轻松地对多个套件使用相同的行为，而没有冗余。让我们创建新的测试套件。

我们已经做好了实施测试的一切准备。那是无痛的！

为了让文章简短易懂，我们不会实现复杂完整的测试。我们将满足于测试请求用户访问我们的存储库返回正确的数据。

使用 go-sqlmock 进行测试的一般过程非常简单:

*   期待一个 SQL 查询。它可以包含参数。这里，这将是我们的用户 ID。
*   创建“假”行，并说我们预期的 SQL 查询将返回它们。
*   执行我们测试过的代码和断言。

让我们运行我们的测试，就像这样，没有任何数据库在后台运行。太好了，成功了！像这样的简单案例的测试将更容易编写，执行起来也更快！

在我们结束这篇文章之前，让我们来谈谈负面因素，因为没有什么是完美的。

*   GORM 可以生成一些复杂的查询。让它与预期的模拟查询完全匹配可能会很棘手。并且在启用调试时复制粘贴您在程序输出中得到的查询不是一个理想的解决方案。
*   在这个例子中，模型非常简单，只有四列。想象一下处理一个有十列或更多列的模型。在生成记录之前，您必须逐个手动复制列名。如果我们可以使用类似于[工厂](https://goyave.dev/guide/advanced/testing.html#using-factories)的东西，那就太好了，但是那样会生成 go-sqlmock 行。可悲的是，没有什么可以满足这种需求…至少现在是这样。

非常感谢您的阅读！如果你还不知道 Goyave，可以在 Github 和官方文档网站上看看[！](https://github.com/go-goyave/goyave)