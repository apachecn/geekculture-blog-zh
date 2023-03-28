# TypeScript 内置类型。第二部分

> 原文：<https://medium.com/geekculture/typescript-builtin-types-part-ii-2059803be7f6?source=collection_archive---------17----------------------->

又来了！在我的[上一篇文章](/geekculture/typescript-bultin-types-part-i-b39d702bb536)中，我们浏览了`Partial<T>`、`Required<T>`、`Readonly<T>`和`Pick<T, K>`类型，今天我们继续用新工具加强我们的武器库。

![](img/c029eda3d2c942dd4eb0fdbf2f491096.png)

开始吧！第一个是`Record<K, T>`:

`K extends keyof any`实际上是指`K`可以是`string`、`number`或`symbol`，`[P in K]: T`是指每个属性都应该是`T`。

我记得我第一次面对`Record<T, K>`时，我已经定义了一个变量为`object`，并收到了 eslint 错误:

当然，我可以禁用这条规则，但我决定找出为什么`object`难以使用。`object`当我们不知道变量的结构时可以使用 type，或者它可以是动态的，如在键值映射中，否则强烈建议为变量定义`interface`或`type`。那么，`object`和`Record<K, T>`有什么区别呢？让我给你看一个例子:

一旦我们初始化了一个`object`，我们就不能向它添加新的属性。但是对于`Record<string, unknown>`来说，这是完全合法的，因为它的定义表明它包含未知类型的属性，并且属性的键是字符串。这就是为什么`Record<K, T>`比较好用的原因。

当然，语句`Record<string, unknown>`很庞大，我建议您为它创建一个类型别名:

我保留了`T`类型参数，以防对象具有相同类型的属性，比如在键值映射中。

在继续之前，我们需要复习一下关于联合类型的知识。

`const u1: number | string`意味着`u1`可以是`number`或者`string`，我们不知道它在编译时的确切类型。你可能注意到了`|`运算符类似于逻辑 or 运算符`||`，有着相同的含义。引入联合类型是因为 JavaScript 是一种非类型化语言，它的一些函数/方法可以根据某些条件返回不同的类型，比如`Array.find`方法:

它返回匹配谓词的第一个元素，如果没有找到匹配，则返回`undefined`。

`Exclude<T, U>`帮助我们处理类型联合。它从`T`中排除了`U`的可能类型。以下是来自官方打字文档的一些例子:

让我们看看它是如何工作的:

这是一个条件类型，它的工作方式类似于代码中的条件语句。`T extends U`是一个谓词，如果谓词为`true`则返回`never`，否则返回`T`。让我们来看看类型转换的所有步骤，并弄清楚它是如何工作的:

引用自[文档](https://www.typescriptlang.org/docs/handbook/basic-types.html#never):*“*`*never*`*类型表示从不出现的值的类型。”。这就是为什么我们在最后一步移除了`never`。它类似于布尔逻辑像`a | false`，在 OR 语句`false`中不影响任何东西，可以去掉。*

实际上`Exclude<T, U>`在日常生活中并不常用。但它被用于下一种类型，这就是为什么理解它是如何工作的如此重要。

下一个是`Extract<T, U>`，和`Exclude<T, U>`很像。让我们简要地回顾一下类型转换的所有步骤:

如你所见,`Extract<T, U>`做了相反的转换。它保留来自`T`的可以分配给`U`的类型，其他类型被删除。如果我们把`Extract<T, U>`和`Exclude<T, U>`连在一起，我们就得到了`T`。这两种类型很容易混淆，它们执行相似但相反的转换，并且它们的拼写非常相似。

现在是今天的主菜`Omit<T, K>`。

![](img/a97e404dd2f3b15b33f329cfc6041aa3.png)

假设您正在编写一个简单的 CRUD 应用程序。实体看起来像这样:

对于`create`函数，我们不需要字段`id`、`createdAt`和`updatedAt`，因为实体尚未创建。这些字段应该在`create`方法本身中设置。那么，我们如何为`create`函数参数定义类型呢？在[上一篇文章](/geekculture/typescript-bultin-types-part-i-b39d702bb536)中我们学习了如何使用`Pick<T,K>`，我们需要像这样的东西，但它应该排除属性，而不是选择。这是`Omit<T,K>`:

除了`id`、`createdAt`和`updatedAt`之外，`UserEntity`的所有属性都可以在`userData`中访问。让我们弄清楚它是如何工作的:

`Exclude`过滤属性键，`Pick`从原始类型`T`中提取过滤后的属性。让我们为`UserEntity`完成所有这些转换:

所以，今天我们学习了如何处理`Record<K, T>`、`Exclude<T, U>`、`Extract<T, U>`和`Omit<T, K>`。你可能对所有这些排除、省略、挑选和提取的东西感到困惑。下面的表格可以帮助你记住这个主题，把它保存在某个地方:

在下一部分，我们将讨论`NonNullable<T>`、`Parameters<T>`、`ConstructorParameters<T>`和`ReturnType<T>`。

下次见，servus！

*   [TypeScript 内置类型部分 I. (](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Partial](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [，](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Required](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [，](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Readonly](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [，](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Pick](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [)](/geekculture/typescript-bultin-types-part-i-b39d702bb536)
*   [TypeScript 内置类型第二部分。(](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)`[Record](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)``[Exclude](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)``[Extract](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)``[Omit](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)`[)](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)
*   [TypeScript 内置类型第三部分。(](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[NonNullable](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[Parameters](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[ReturnType](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[ConstructorParameters](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)`， `[InstanceType](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [)](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)