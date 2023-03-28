# 类型脚本布尔丁类型。第一部分

> 原文：<https://medium.com/geekculture/typescript-bultin-types-part-i-b39d702bb536?source=collection_archive---------17----------------------->

大家好。今天，我们将讨论一些有用的 TypeScript bultin 类型，它们可以极大地简化您的代码并实现某种类型魔法。

![](img/18387364352ca7b22a57dd66c2cece47.png)

让我们从著名的类型`Partial<T>`开始:

对于具有`T`所有属性列表的对象，我们可以使用`Partial<T>`或者只对其中的一部分使用`Partial<T>`。一个空的物体`{}`也可以用作`Partial<T>`。但是我们不能传递属性不存在于`T`中的对象。

`Partial<T>`是一个非常有用的工具，我们可以用它来进行数据库更新查询或者在 http `PATCH`方法体中，当我们不整组对象属性的时候。现在让我们弄清楚它是如何工作的:

正如您所看到的，它的类型定义非常简单，只有 3 行代码。`[P in keyof T]`限制`Partial<T>`只能拥有原始类型`T`的属性，这就是为什么我们在第 11 行和第 14 行收到错误。`T[P]`检查属性的类型与原始类型的属性相同，这就是第 17 行错误的原因。`Partial<T>`的所有魔法都是由一个字符`?`完成的，它标志着`T`的所有属性都是可选的。因此，`Partial<T>`会执行以下操作:

This is an example of TypeScript powerful and flexible type system.

很酷，不是吗？好的，下一步是什么？

`Required<T>`做相反的转换，它标记了`T`所有需要的属性:

`Partial<T>`定义大同小异，只有一个区别:`?`被`-?`代替。该操作符从属性中删除可选标志`?`。

老实说，我不确定`Required<T>`是否非常有用，但是也许有一天我会面对它的真实用例。

下一种类型是`Readonly<T>`

`Readonly<T>`将所有`T`属性标记为只读。很多人认为它没用，但是它可以帮你避免意想不到的副作用，写出[纯函数](https://en.wikipedia.org/wiki/Pure_function)。例如在该 [Redux](https://redux.js.org/) 功能中:

从 TypeScript 的角度来看，一切都是合法的，但对于 Refux 方法来说，这是完全错误的。我们不能像第 7 行那样改变 reducer 中的状态。接下来是`Readonly<T>`:

简单地添加`Readonly`会限制状态的任何变化，并迫使我们创建一个新对象，而不是修改旧对象。如果你是函数式编程的忠实粉丝，你可以用`Readonly<T>`来包装所有的函数参数，使得所有的函数都是纯函数。

假设我们为 REST API 编写客户端，我们需要为 API 调用选项创建公共接口，并为所有 http 方法`GET`、`POST`、`PUT`、`DELETE`创建接口

对于`GET`方法，我们不需要`body`，对于`POST`方法，我们不需要`query`，因为所有数据都在`body`中传递，以此类推。如您所见，这段代码包含了大量重复的属性声明。如果属性的类型改变或者添加了新的属性，保持它们都是最新的将是一件令人头疼的事情。我们能做点什么吗？当然，对于这种情况，TypeScript 提供了`Pick<T, K>`:

`Pick<T, K>`比前面的类型稍微复杂一点，它包含了第二个类型参数`K`。`K extends keyof T`表示`K`可以是属性键之一，如下:

由于限制`K extends keyof T`，我们只能选择在原始类型中定义的属性。我们从`RESTApiRequestOption`中提取了`url`属性，并继承了它的类型。所有其他属性都被忽略，不能使用。有意思，但是只提取一个属性用处不大。我们如何挑选两个或更多的属性？也可以使用类型联合:

我们没有通过 T20，而是通过了另一个魔术般的联盟`'url' | 'method'`。

![](img/4fe90f5019b7f4bacefd1a9746b95ae5.png)

当 TypeScript 编译器迭代`RESTApiRequestOptions`属性键时，根据指令`[P in K]`检查`'url' | 'method'`中的属性键是否存在。`'url'`和`'method'`都在`'url' | 'method'`，这就是它的工作方式。现在让我们用`Pick<T, K>`重写我们的例子:

神奇吧。我们消除了重复并减少了代码。现在，如果我们更改原始接口中的某些属性类型，它将应用于所有其他类型。

TypeScript 为我们提供了灵活的类型机制和强大的实用类型。我希望这篇文章能帮助你理解它的工作原理和使用方法。欢迎在评论中提出任何问题。

今天到此为止。这个话题相当大，我决定把它分成几个部分。在下一篇文章中，我们将讨论`Record<K, T>`、`Exclude<T, U>`、`Extract<T, U>`和`Omit<T, K>`。

下次见，servus！

*   [TypeScript 内置类型部分 I. (](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Partial](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [，](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Required](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [，](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Readonly](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [，](/geekculture/typescript-bultin-types-part-i-b39d702bb536) `[Pick](/geekculture/typescript-bultin-types-part-i-b39d702bb536)` [)](/geekculture/typescript-bultin-types-part-i-b39d702bb536)
*   TypeScript 内置类型第二部分。( `[Record](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)` [，](/geekculture/typescript-builtin-types-part-ii-2059803be7f6) `[Exclude](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)` [，](/geekculture/typescript-builtin-types-part-ii-2059803be7f6) `[Extract](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)` [，](/geekculture/typescript-builtin-types-part-ii-2059803be7f6) `[Omit](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)` [)](/geekculture/typescript-builtin-types-part-ii-2059803be7f6)
*   [TypeScript 内置类型第三部分。(](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[NonNullable](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[Parameters](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[ReturnType](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[ConstructorParameters](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [，](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482) `[InstanceType](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)` [)](https://luckylibora.medium.com/typescript-builtin-types-part-iii-e2b75107c482)