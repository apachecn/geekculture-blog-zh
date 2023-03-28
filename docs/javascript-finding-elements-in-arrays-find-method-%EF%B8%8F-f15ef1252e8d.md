# JavaScript 在数组中查找元素— Find()方法⭐️💥

> 原文：<https://medium.com/geekculture/javascript-finding-elements-in-arrays-find-method-%EF%B8%8F-f15ef1252e8d?source=collection_archive---------15----------------------->

作为开发人员，我们需要定期访问数组中的元素并与之交互。 [**Map()**](/nerd-for-tech/javascript-mapping-arrays-map-method-a650c014fe7) 和 [**Filter()**](https://javascript.plainenglish.io/javascript-filtering-arrays-filter-method-ac582e4594e2) 方法都是我们需要根据某种逻辑变换数组元素或者寻找符合某种条件的元素时的好选择，而且这两种方法都返回一个新的数组。[**【Reduce()**](/geekculture/javascript-reducing-arrays-reduce-method-a211e9b82804)方法将数组元素列表缩减为一个单一的值。

*当我们需要定位数组中的单个元素并返回该元素时，编程世界中另一个非常常见的任务是什么？*

![](img/4521b945b435badb33fc7b7069522443.png)

Photo by [Johny vino](https://unsplash.com/@johnyvino?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，我们可以使用两种不同的方法来定位数据或数组中的单个元素——对于简单明了的情况，我们可以使用 **IndexOf()** 方法，而要使用更复杂的计算来查找元素，我们使用 **Find()** 方法。现在让我们深入了解一下 **Find()** 方法吧！

作为一个好的起点，让我们在 [**MDN Web Docs 网站**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) 上参考一下 **Find()** 方法的官方文档和定义:

> Find()方法返回所提供数组中满足所提供测试函数的第一个元素的值。如果没有满足测试函数的值，则返回 undefined。
> 
> 返回值。满足所提供测试函数的数组中第一个元素的值。否则，返回 undefined。

注意这个方法返回符合条件的第一个 元素(value，而不是 index)。这意味着如果数组中有多个重复的数据/相同的值，那么只返回第一个。

在下面的例子中，我创建了一个`fruits` 数组，并用各种类型的水果填充它。比方说，我想在这个数组中找到一个长度为 6 个字母的元素。让我们先看看如何手动解决这个问题，然后通过应用 **Find()** 方法再解决一次同样的问题。试试吧！

```
function findMethodDemo(arrayOfFruits) {for (var i = 0; i < arrayOfFruits.length; i++) {
if (arrayOfFruits[i].length === 6) {
return arrayOfFruits[i];
}}}const fruits  =
[
"Fig",
**"Apples",** "Blueberries",
**"Grapes",** "Apricot",
"Kiwi",
"Strawberry"
];const result = findMethodDemo(fruits);
console.log(result);// => **Apples** 
```

它是如何工作的？我们遍历数组`fruits` ，如果数组中当前元素的长度等于我们在`arrayOfFruits[i].length === 6`之前定义的条件，那么我们返回该元素，否则将返回`undefined`。请注意，尽管有两个值**" apple "**和 **"Grapes"** 具有相同的长度 6，但只返回第一个，因为一旦找到第一个匹配元素，一个`return` 关键字就会停止函数执行，这样 JavaScript 机器就永远不会到达第二个字 **"Grapes"** 。

当我们在`fruits` 数组上调用 **Find()** 方法时，同样的逻辑也适用，请看下面！

```
const fruits  =
[
"Fig",
**"Apples",** "Blueberries",
**"Grapes",** "Apricot",
"Kiwi",
"Strawberry"
];const result = fruits.**find**(element => element.length === 6);
console.log(result);// => **Apples**
```

方法自动循环访问数组，对该数组的每个值调用回调函数，并返回数组中满足该函数定义的条件的第一个元素。如果没有匹配的元素，则返回`undefined` 。

# ⭐️💥**Find()方法的语法**💥⭐️

我们已经学习了这个 **Find()** 方法是如何工作的，所以让我们来看看语法，并通过测试和更多的练习来检验我们的知识！下面是[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)**的官方文献告诉我们的:**

```
// **Arrow function**find((element) => { ... } )
find((element, index) => { ... } )
find((element, index, array) => { ... } )// **Callback function**find(callbackFn)
find(callbackFn, thisArg)// **Inline callback function**find(function callbackFn(element) { ... })
find(function callbackFn(element, index) { ... })
find(function callbackFn(element, index, array){ ... })
find(function callbackFn(element, index, array) { ... }, thisArg)
```

**我们可以使用一个**箭头函数表达式**、**回调函数**或**内联回调函数**来定义和使用 **Find()** 方法。我们已经编写了箭头函数表达式，现在让我们重新编写相同的代码来演示回调函数和内联回调函数。无论选择哪种语法，输出都是一样的；这里有几个例子。**

****内嵌回调函数。****

```
const fruits  =
[
"Fig",
**"Apples",** "Blueberries",
**"Grapes",** "Apricot",
"Kiwi",
"Strawberry"
];const result = fruits.**find**(function (element) {return element.length === 6});
console.log(result);// => **Apples** 
```

****回调函数。****

```
const fruits  =
[
"Fig",
**"Apples",** "Blueberries",
**"Grapes",** "Apricot",
"Kiwi",
"Strawberry"
];function **callBack**(fruits) {
return fruits.length === 6;}const result = fruits.**find**(callBack);
console.log(result);// => **Apples** 
```

# **⭐️💥**对象呢？**💥⭐️**

**现在，让我们将我们的原始数组转换成对象的*数组，看看我们如何将 **Find()** 方法应用于更复杂的数据结构。同样，让我们在这个修改后的数组中添加重复的数据/相同的值，以证明只返回第一个匹配的值:***

```
const fruits  =
[
{fruit: "Fig", count: 10},
{fruit: "Apples", count: 5},
{fruit: "Apples", count: 50},
{fruit: "Blueberries", count: 123},
{fruit: "Grapes", count: 7},
{fruit: "Apricot", count: 45},
{fruit: "Kiwi", count: 73},
{fruit: "Strawberry", count: 11}
];
```

**尝试这段代码，看看输出是什么:**

```
const fruits  =
[
{fruit: "Fig", count: 10},
{fruit: "Apples", count: 5},
{fruit: "Apples", count: 50},
{fruit: "Blueberries", count: 123},
{fruit: "Grapes", count: 7},
{fruit: "Apricot", count: 45},
{fruit: "Kiwi", count: 73},
{fruit: "Strawberry", count: 11}
];const result = fruits.**find**(findfruit => findfruit.fruit === "Apples");console.log(result);// => **{ fruit: 'Apples', count: 5 }**
```

**![](img/a66d920780bb7d07629ffae912aefc35.png)**

**Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

# **⭐️💥如何使用**索引？**💥⭐️**

**该方法的另一个可选参数是一个**索引**，**索引选项/参数**允许您根据元素的索引定位并获取元素的值。例如，我们希望 **Find()** 方法在`index 0`定位一个元素并返回它的值。下面的例子返回 **"Fig"** ，因为众所周知数组是基于 0 索引的。**

```
const fruits  =
[
**"Fig",**
"Apples",
"Blueberries",
"Grapes",
"Apricot",
"Kiwi",
"Strawberry"
];const result = fruits.**find**((element, **index**) => {return **index === 0**});console.log(result);//  => **Fig**
```

**或者我们想定位另一个元素，它的长度是 6 个符号，从`index 1`开始，直到第一个匹配，我们可以这样写代码。下面这段代码返回 **"Grapes"** ，因为 JavaScript 从`index 1`开始迭代这个数组，即**" apple "**，直到第一个匹配 **"Grapes"** ，然后将返回第一个匹配。**

```
const fruits  =
[
"Fig",
**"Apples",**
"Blueberries",
**"Grapes",**
"Apricot",
"Kiwi",
"Strawberry"
];const result = fruits.**find**((element, **index**) => {if (element.length === 6 && index > 1) return true});console.log(result);// => **Grapes**
```

****对象的索引参数。****

**同样的逻辑适用于*对象数组*；下面是在`index 2`返回一个对象的代码:**

```
const fruits  =
[
{fruit: "Fig", count: 10},
{fruit: "Apples", count: 5},
**{fruit: "Apples", count: 50},**
{fruit: "Blueberries", count: 123},
{fruit: "Grapes", count: 7},
{fruit: "Apricot", count: 45},
{fruit: "Kiwi", count: 73},
{fruit: "Strawberry", count: 11}
];const result = fruits.**find**((element, **index**) => {return **index === 2**});console.log(result);// => **{ fruit: 'Apples', count: 50 }**
```

# **⭐️💥**我们的外卖是什么？**💥⭐️**

**对于更复杂的数组搜索， **Find ()** 方法既简单又有用。该方法自动遍历给定的数组，对该数组的每个值调用回调函数，并返回数组中满足回调函数定义的条件的第一个元素。如果没有元素符合条件，则返回 undefined。**

**![](img/46bf5c109106b850aa5945c8114cb887.png)**

**Photo by [Timon Klauser](https://unsplash.com/@timon_k?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

*   ****Find()** 方法在一个数组上被调用。**
*   ****Find()** 方法以函数为自变量。这使我们能够定义元素应该满足的条件，允许更复杂的搜索。**
*   ****Find()** 方法返回数组中满足条件的第一个元素。**
*   ****Find()** 方法返回满足条件的元素的值。**
*   ****Find()** 如果什么都没有找到/如果没有匹配的元素，方法返回 undefined。**

**如果你觉得这些信息有用，请随时关注我！希望您喜欢这篇关于 JavaScript 中 Find()方法的简短指导，请保持关注。**