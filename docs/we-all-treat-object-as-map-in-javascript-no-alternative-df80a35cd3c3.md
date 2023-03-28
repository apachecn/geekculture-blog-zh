# Javascript 里我们都把 Object 当 Map，没得选？

> 原文：<https://medium.com/geekculture/we-all-treat-object-as-map-in-javascript-no-alternative-df80a35cd3c3?source=collection_archive---------34----------------------->

![](img/983a6b1133481def051f59d679738246.png)

在包括 javascript 在内的不同语言中，“map”是作为一个高阶函数引入的，它使用另一个函数将任何结构(如数组)转换成某种形式。但是当我们谈论一个著名的数据结构“Map”时，情况就不一样了。Map 是一种非线性数据结构，我们经常使用它来满足特定用例下的特定目的。许多编程语言为使用 map 提供了内置支持，可能名称不同。例如，在 python 中，这被称为充当地图的“字典”。不幸的是，在 Ecmascript2015 发布之前，Javascript 没有名为“Map”的东西。这种缺失迫使开发人员养成了在需要时使用“对象”作为“地图”的习惯。这篇博客假设你已经知道 javascript(并且肯定熟悉 object ),并且已经像其他程序员一样毫不犹豫地使用 Object 作为 Map。在大多数情况下，使用对象作为地图并没有致命的问题，写这篇博客也不是为了劝阻你使用它。相反，我想展示在某些用例中使用 object 作为 map 的一些异常，以及如何通过遵循一些技巧或使用内置的 Map 类来避免这些异常。

Javascript 对象在许多方面方便了程序员的任务。最重要的一点是，你不必为了在程序中随时利用对象的能力而创建一个类。例如，如果您必须在单个请求中向服务器发送一组数据，您可以简单地创建一个具有相关属性的对象，这些属性包含这些值。在其他编程语言中，创建一个类是在使用它的对象之前必须做的事情，这可能是一个冗长的任务。例如

```
let person = { name : "reza", age : 25, occupation : "student" }
```

我们可以看到，人是一个具有“姓名”、“年龄”和“职业”属性的客体。我们可以一次就知道每个房产的价值。

```
console.log(person.name) // returns "reza" 
console.log(person.age) // returns 25
```

使用 object.property 是许多编程语言中访问对象属性的常用方法。Javascript 提供了另一种访问属性的方法，就好像它将属性视为“键-值”对一样。举个例子，

```
console.log(person[“name”] ) *// returns “reza”*
```

由于 js 对象具有如此大的灵活性，每当我们需要使用类似 map 的结构时，我们通常将它视为“Map”。就其本质而言，Javascript 是一种棘手的语言，如果你没有足够的好奇心去看它的异常行为，它会让你保持平静[但是当生活开始展现出月亮的阴暗面时，它就像地狱一样]。我们现在将使用前面提到的例子来展示使用 javascript object 作为 Map 的一些不一致性。

```
console.log("name" in person) // true console.log("address" in person) // false . person has no such property. console.log("toString" in person) // true . but why?
```

前两个例子工作正常，但是在第三个例子中，尽管“person”对象没有像“toString”那样的键，它还是返回 true。太奇怪了，对吧？

让我们试着理解这种不一致背后的原因？“in”操作符检查给定的属性在对象中是否有效。Javascript 实际上是一种基于原型的语言，每个对象都有一个类似“原型”的东西(实际上是对象),它记录了从“主/父”对象/类继承的所有“它的”属性。这就是通过原型链接在 js 对象中确保继承的方式。您可以在浏览器的控制台中使用 console.dir("any object ")来查看原型链接。

很明显，javascript 中的每个对象都继承了“主”对象的所有东西。为此，我们可以对字符串、数组使用一些外来的方法，甚至不用在代码中定义它们。“toString”就是这样一个方法，每个对象都从主对象继承而来。尽管“person”对象没有像“toString”那样的键，但它是在它的 master 对象中定义的。我想，现在你已经明白为什么它在最后一个例子中返回“真”了。

有什么办法可以从我们的对象中摆脱这种不一致？答案是肯定的。你可以简单地告诉 javascript 我的对象没有任何原型。如果您通过以下方式创建对象

```
Object.create(null)
```

创建的对象将没有原型以及那些“外来的”属性和方法。为了理解这一点，让我们详细阐述一下。

```
let person = Object.create(null) 
console.dir(person) // it will show no properties in it .
```

与前一个不同，创建的对象不会从主“对象”继承任何东西。要进行检查，请在浏览器的控制台中打开它。现在让我们尝试添加我们希望在 person 对象中看到的属性。

```
person.name = "reza" 
person.age = 25 
console.dir(person) // it will just show name and age as property.
```

现在，如果你检查当前对象中不存在的任何东西，它将显示 false。喔喔喔！

将对象视为地图还有其他问题。例如，对象的每个属性都应该是一个字符串。因此，在从“键”到“字符串”的转换很困难的用例中(在对象和函数的情况下)，您不能使用对象作为映射。别担心！！Javascript 提出了一个名为 Map 的新结构(与 Ecmascript 2015 一起发布),这正好满足了我们的目的。为什么不用这个？

```
 let person = new Map() 
 person.set("name","reza");
 person.set("age",25);
 person.has("name");// returns true
 person.has("age") ; // returns true
 person.has("toString") ;// returns false
```

首先，我们创建了人物地图。然后，我们为 person 设置两个属性(姓名和年龄)。然后，我们用一些键检查人员图，这给了我们预期的结果。记住一件事，如果你试图使用“in”操作符来查看任何在 map 内部有效的东西，你将会看到一些不一致。举个例子，

```
console.log("toString" in person) // still returns true
```

我们永远不会使用“in”运算符来检查地图内部的内容。因为，它检查的不是地图上的东西，而是物体本身。显而易见，person 本身就是一个引用 Map 类创建的对象。因此，“toString”和 prototype 中的其他键在 person 对象中仍然有效。但是如果你用“has”方法检查它们，它们将无效并返回 false。

我们还可以用“get”方法访问属性/键的值。

```
person.get("name") // returns "reza"
```

以下是使用地图的其他优势:

*   它保留了我们设置的键的顺序，而在 object 的情况下却不能保证。
*   我们可以很容易地找到地图的大小与大小的方法，但它不是那么简单的对象。

最后但并非最不重要的，地图不是使用对象作为地图的替代品。您仍然可以将我们可爱的“对象”视为遵循特定规则的地图。例如，Object.keys("myobject ")将只返回 myobject 的键。使用“in”运算符的另一种替代方法是“hasOwnProperty”方法，因为它在检查时不会考虑继承的属性。综上所述，我只是试图阐明物体和地图的不同方面。在不同的用例中使用哪一个取决于你。编码快乐！！

*参考:口才 Javascript，MDN。*