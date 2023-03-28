# 如何创建自定义的 Typescript 装饰器

> 原文：<https://medium.com/geekculture/how-to-create-a-custom-typescript-decorator-c4a1998e1b5e?source=collection_archive---------2----------------------->

![](img/a6f9f6b99d7c15a1ba61ec2c672fb62c.png)

Image by [Jill Wellington](https://pixabay.com/users/jillwellington-334088/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=553457) from [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=553457)

如果您已经使用 Typescript 有一段时间了，那么您很可能已经在代码中看到并使用了 decorators。它们是应用于类、方法、属性或方法参数的函数，使用符号`@`后跟装饰函数名。

## 它们是用来做什么的？

TypeScript 装饰器是 TypeScript 的一项功能，它允许您将元数据附加到类、方法和属性，并在被装饰的元素被访问或修改时执行逻辑。它们是在运行时修改类或其成员的行为的好方法，而不改变类本身的实现。

它们可用于多种任务，例如:

*   向类或其成员添加附加行为
*   实现依赖注入
*   修改第三方库，而不必改变库本身的实现。

## 装潢师的组成部分

在本文中，我们将创建一个`memoize`方法装饰器，它将帮助我们缓存函数调用的输出，以更好地优化我们的代码。

在我们开始之前，让我们详细看看方法装饰器的组件:

装饰函数有三个参数:

1.  `target`:这是正在应用装饰器的对象。在方法装饰器的情况下，它将是该方法所属的类的原型。
2.  `key`:这是被修饰的属性或方法的名称。在方法装饰器的情况下，它将是方法的名称。
3.  `descriptor`:这是一个包含修饰属性或方法信息的对象，比如它的值和任何其他属性。

## 我们开始吧！

现在我们对装饰器的组成有了一个大致的概念，让我们从定义我们的`memoize`装饰器开始。`memoize`的目的是缓存之前函数调用的结果，并将之前的结果用于具有相同参数的后续调用。为此，我们将使用一个`Map`对象在内存中存储参数和输出的键值映射。

接下来，我们将修改`descriptor`对象的`value`属性，将原始方法包装在一个新函数中，该函数将返回缓存的值(如果存在的话),并将新的函数调用结果存储在缓存中。最后，我们返回修改后的`descriptor`对象。

我们现在已经完成了自定义装饰。下一步将是测试它！

让我们用 Fibonacci 序列算法的递归实现创建一个类，并向其中添加我们的装饰器。

运行这段代码，您应该会在控制台上看到以下输出:

```
1
1
2
3
5
8
13
21
34
55
89
144
```

完美，你也可以在没有`memoize`装饰器的情况下运行代码来检查，那将需要很长时间来完成。

## 有什么不好的地方吗？

您应该知道 TypeScript decorators 有一些限制。首先要注意的是，它们不是 Javascript 语言的一部分，所以只能在 Typescript 中使用它们。

创建自定义装饰器时要记住的另一件事是将它们用于相对简单和直接的任务，因为它们可能不太适合以容易理解或可预测的方式修改类或方法的行为。

## 结论

好了，这篇文章就到这里。我希望您获得了一些关于 Typescript decorators 的见解，以及如何在您的下一个项目中利用它们。你可以在这里找到完整的源代码[。感谢您的阅读！](https://replit.com/@eyuelberga/TypescriptDecoratorDemo?v=1)