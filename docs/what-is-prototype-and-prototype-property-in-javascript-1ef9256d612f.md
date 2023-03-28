# JavaScript 中的[[Prototype]]和 Prototype 属性是什么？

> 原文：<https://medium.com/geekculture/what-is-prototype-and-prototype-property-in-javascript-1ef9256d612f?source=collection_archive---------7----------------------->

# 目标

理解 JavaScript 中的`[[Prototype]]`和`prototype`属性的区别。

# 让我们开始…

JavaScript 是一种高级、多范式、单线程编程语言。

除了它的许多特性之外，它还支持基于原型的面向对象。这意味着我们可以为对象提供一个蓝图，这样就有了相同类型的对象所共有的属性或方法。

在其他编程语言中，这种特性被称为**类继承**。但在 JavaScript 中，它被称为**原型继承**。

> 在下文中，单词**原型**在多个上下文中使用。
> 
> 要理解上下文，请遵循下面提到的指南—
> 
> *如果只提到了`prototype`，那么它意味着一个对象的蓝图。
> 
> *如果提到了`prototype property`，那么它意味着一个函数的属性，这个函数的关键字是**原型**。

## **#1)什么是[[原型]]属性？**

`[[Prototype]]`是对象中的一个属性，指向对象的蓝图或原型。

让我们举个例子——

Gist #1.1

在上面的代码中，我们试图将`indiaCountryObj`设置为`haryanaStateObj`和`gujaratStateObj`的原型，使用 Object 类的静态`**setPrototypeOf()**`方法。

让我们看看下面的输出—

Video #1.1

我们看到使用`Object.setPrototypeOf()`方法设置的原型被放置在`[[Prototype]]`属性中。

现在，访问对象的属性值的方式是—

*   首先，它将在主对象中寻找属性。
*   如果在主对象中没有找到该属性，它将在该对象原型中查找该属性。
*   如果它不存在，那么它将查看主对象的原型的原型。并且继续下去，直到找到属性值，否则返回 undefined。

所以，如果我们做这样的事情—

```
haryanaStateObj.country // India
```

然后首先检查`haryanaStateObj`的`country`属性。因为它不在那里，所以它继续查看`haryanaStateObj`的原型，并在那里找到`country`属性。

为了更清楚地说明这一点，考虑下面的例子—

Gist #1.2

Gist #1.2 中第 12 行的输出是“德国”，因为在`haryanaStateObj`中寻找属性，并且在那里找到该属性本身。

您可能想知道，如果`[[Prototype]]`是一个属性，那么我们可以通过执行下面的命令来访问该对象的原型吗

```
haryanaStateObj.[[Prototype]]
```

答案是**没有**！！

为了得到一个物体的原型，你必须做一些事情，如下面的视频所示

Video #1.2

所以，简而言之，你将不得不使用以下方法—

*   object . getprototypeof(haryanasteobj)；
*   哈里亚纳邦 Obj。__proto__

## #2)什么是原型属性？

如果我们想要创建 100 个具有相同原型的对象，仅仅使用第 1 节中提到的方法来设置对象的原型是不正确的。需要另一种方法来达到这个目的。

我们可以使用函数构造器，借助函数中的`prototype`属性来制作这样的对象。

> 在进一步讨论之前，请注意原型属性**仅在函数构造函数的情况下**才能按预期工作。

首先让我们用函数构造器创建一些对象，而不设置它的 prototype 属性。看看下面的代码。

Gist #2.1

现在，让我们看看输出——

Video #2.1

通过查看通过函数构造器创建的对象的结构，我们看到`[[Prototype]]`属性将构造器属性和对象作为原型的原型。

> 这意味着当前函数的原型属性拥有构造函数本身。

现在让我们在 prototype 属性中添加一些属性。

Gist #2.2

这部分很有意思。

Video #2.2

我们在`getId.prototype`下设置的所有属性现在都是所创建对象的`[[Prototype]]`的一部分。

# 结论

在这里，我们看到了设置对象原型的方法，并且理解了`[[Prototype]]`和`prototype`属性之间的区别。

`prototype`是函数构造器的属性，它在对象的`[[Prototype]]`中设置所需的属性。

# 参考

1.  [https://en.wikipedia.org/wiki/JavaScript](https://en.wikipedia.org/wiki/JavaScript)
2.  [https://medium . com/@ venkatiyengar/proto-vs-prototype-d3c 9df 933 f 58](/@venkatiyengar/proto-vs-prototype-d3c9df933f58)
3.  [https://JavaScript . plain English . io/proto-vs-prototype-in-js-140 B9 B9 c8 CD 5](https://javascript.plainenglish.io/proto-vs-prototype-in-js-140b9b9c8cd5)