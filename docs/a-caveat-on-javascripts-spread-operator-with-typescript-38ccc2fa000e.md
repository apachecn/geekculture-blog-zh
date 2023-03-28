# 使用 TypeScript 对 JavaScript 的 spread 操作符进行警告

> 原文：<https://medium.com/geekculture/a-caveat-on-javascripts-spread-operator-with-typescript-38ccc2fa000e?source=collection_archive---------34----------------------->

让我们从这段代码开始这篇文章。

![](img/f7c26b8a6b25545e05b5465b1c7c9ebc.png)

Function parameter overloaded

对`bodyMassIndex`的两次调用都不会在 TypeScript 上抛出任何警告，即使第二次调用在键`name`中有一个重载的参数。关于 TypeScript 的打字有一点，就是*没有*那么严格。

例如，TypeScript 中的合同或`interface`可以有可选属性。此外，它的具体实现允许包含比`interface`定义的更多的属性。有些人可能会说这是签订合同的错误方式，但对我来说，至少比没有合同要好。

回到岗位上。

我已经看到[扩展操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)在我工作中维护的一个项目中被广泛使用。通常，这在映射从应用程序外部传递的一些参数时使用。例如，来自客户端应用程序的 JSON 有效负载最终可能会修改数据库级别的内容。这是应该添加警告的地方。

正如这篇文章的介绍中所强调的，在 TypeScript 中键入只是确保您传递的值至少满足类型。如果你的对象值中有更多的属性，没问题！嗯，技术上没有问题。从商业角度来说，你可能很难找到问题。

想象一下，JSON 有效负载包括一些您没有预料到的其他值。假设它包含一个`ownerId`字段，指示谁可以访问资源。如果代码盲目地将有效负载分配给存储库调用，任何人现在都可以更改资源所有权。

这个例子可能是人为的，但我确实在我工作的生产级应用程序中看到了很多传播，这可能有一天会发生。

那我们该怎么办？

我的主要建议是**每次都要明确**。当然，这是有代价的。对于维护人员来说，您的代码可能过于冗长。如果你使用多层架构，那么当在层之间传递值时，你必须是明确的；这会产生大量的映射函数。对我来说，好处大于所需的努力。

补救这种情况的另一种方法是每次都验证价值。对于对象，您可能会从像[类验证器](https://www.npmjs.com/package/class-validator)这样的包中获得一些帮助。这可能没有前一个冗长。所以这对你来说可能是个不错的选择。