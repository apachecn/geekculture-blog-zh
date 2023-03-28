# Javascript 对象和数组中的 Const vs Object.freeze

> 原文：<https://medium.com/geekculture/const-vs-object-freeze-in-javascript-905c45b95805?source=collection_archive---------12----------------------->

![](img/275649f6637606647a3850fc91d4d854.png)

Const 创建一个值的只读引用，即分配给 const 变量的值不能被**重新分配**。

## 但是等等…这里有一个陷阱！

现在，我们在上面读到的适用于原始数据类型，即字符串、数字、布尔、空、未定义，但这不适用于非原始数据类型，如对象和数组。

让我们通过一个简单的例子来理解这一点，我们将为每种数据类型创建一个 const 类型的变量，看看我们是否能够改变它的值。

![](img/5713846824258fbfb135a6847c243b20.png)

现在让我们看看当我们试图改变每个原始数据类型变量的值时会发生什么

![](img/79aa9cc701f6aee1f1bbc8fafd0901d6.png)![](img/e1dc54122925f67ae102a7d6a7d46c3c.png)![](img/67a79b92dae9856ec907d53706188278.png)![](img/ff72359f0e4e15a37bad3f35435a3113.png)![](img/25d16d81976a7a40f8ca2e76001efc9b.png)

**我们得到一个错误，指出不能给常量变量赋值，我们完全同意**

**让我们尝试同样的对象:**

如果我们直接将 const 对象重新赋值给其他对象，那么显然它不允许我们这样做，我们会得到相同的赋值给 const not allowed 错误。

![](img/a979e62fd8e707bdaac29e4848edcd57.png)

**控制台输出:**

![](img/bbbaa627ba730fdf8c751081d41d7726.png)

但是当我们对一个对象的属性和一个数组的元素做一些改变时会发生什么呢？让我们尝试一下，我们更改了 personDetails 对象的名称和年龄值，并控制该对象，即使该对象是 const 类型，我们也能够更改该对象的值。

![](img/217851fb4ccd41cca55295c151d2d3cc.png)

**控制台输出:**

![](img/a17d34a011a2c29f39aa2792fe3693f5.png)

## **这里发生了什么？**

根据 const 的定义，我们不能给 const 变量重新赋值，但是在 object & array 类型的情况下，当你添加/改变一个对象或数组时，你不用重新赋值或重新声明常量，它已经被声明和赋值了，你只是改变了键值，并在这里添加了一个新元素。

## 使用 Object.freeze 禁止对象和数组中的更改

如果我们希望我们的对象和数组值不变，那么我们应该使用***Object . freeze(your-Object)来冻结对象。***

因此，在我们的代码中，如果我们使用 Object.freeze(personDetails)并试图更改对象的值，我们将无法这样做

![](img/81b4de3012310098d3d41c6a2b92fedf.png)

现在，当我们看到控制台语句的输出时，我们看到 object 值没有改变，即使在我们尝试改变 object 的值之后，name 仍然是 Muhammad，age 仍然是 17。

**控制台输出:**

![](img/5cfeb92e3a8db61ea2a550998e9742ba.png)

**冻结数组避免变化:**

同样的概念也适用于数组，冻结数组也不允许在数组中修改，当我们试图在冻结它之后将一个元素推入 friends 数组时，我们得到一个错误，指出对象(数组)是不可扩展的。

![](img/bd231fe313fd4c6161097a3342e5e880.png)

**控制台输出:**

![](img/93d39dcb7fb6a1d581de9a95eef9cdab.png)

如果你觉得这篇文章有帮助，请鼓掌👏并分享给你的朋友。

***每天学点新东西***