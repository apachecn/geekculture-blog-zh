# JavaScript 初学者入门

> 原文：<https://medium.com/geekculture/getting-started-with-javascript-for-beginners-d2e270493124?source=collection_archive---------27----------------------->

![](img/673a7faba3c832bbe2facf02c8d46822.png)

# 介绍

JavaScript 是初学者入门最好的编程语言之一。有时网上找到的资源并不完全适合初学者，所以在这篇文章中，我将尽可能让你容易地掌握它。

让我们从定义开始。根据 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) ，

> JavaScript 是一种多范式的动态语言，具有类型和操作符、标准的内置对象和方法。

别害怕，让我们把它分解成一个简单的格式。

JavaScript 是一种多范式，[范式](https://en.wikipedia.org/wiki/Programming_paradigm)是一种基于面向对象、函数等特性对编程语言进行分类的方法，多范式简单地说就是这种编程语言可以通过多种特性进行分类。

[动态语言](https://en.wikipedia.org/wiki/Dynamic_programming_language)是一种高级编程语言，支持运行时操作，而静态编程语言只支持编译时操作。这些操作可以是添加新代码、扩展对象、改变类型等。

在 sort 中，JavaScript 数据类型定义了变量中可以存储或修改的数据类型。Ex 编号:1，2，3 数组:[5，10，15]字符串:` Hi`。运算符是定义需要完成哪个操作的符号。例如，数学运算符+(加号)将两个或多个变量相加。

内置对象预定义的代码集，有助于扩展语言的灵活性。许多操作都是由它们完成的，我们可以直接使用它们，而不用编写代码。例如日期、数学、字符串、数组

最后，JavaScript 方法是内置函数，一旦被调用就会返回特定的值。示例 isNaN()检查给定值是否为空。

与 C/C++或 Java 相比，用 JavaScript 编码相对容易。在赋值之前不需要指定数据类型，例如 C : int a = 50 中的
;，在 JS var c = 50/`50`/{number:`50`}中；
JavaScript 中有许多内置方法可以帮助编写代码。让我们来看看 Js 中一些常见的内置方法和函数。

# 常见的内置方法和函数

**字符串类型:**

字符串对于以文本形式保存数据很有用。有一些非常简单的字符串方法可以帮助我们节省大量的时间和代码。要创建字符串，只需键入:

```
//usig variables
const myName = 'Ahsan';//using object
const myName = new String('Ahsan');
```

要进行比较，在 JavaScript 中添加字符串只需使用条件运算符:

```
const str1 = 'Hi';
const str2 = 'Hi';console.log(str1 == str2); // The result is true
console.log(str1 > str2); // The result is falseconsole.log(str1 + str2); // The result is 'HiHi'
```

要将文本转换为大写，只需使用{string.toUpperCase()}方法，类似于对小写应用{string.toLowerCase()}方法。有一个叫做{string.trim()}的方法，用来去除字符串两边的空白。您也可以使用{string.repeat(count)}来重复您的字符串给定的计数。

```
// transform uppercase --> {string}.toUpperCase()const stringLowerCase = 'sky is blue';
console.log(stringLowerCase.toUpperCase()); // returns 'SKY IS BLUE'// transform lowercase  --> {string}.toLowerCase()const stringUpperCase = 'SUN IS RED';
console.log(stringUpperCase.toLowerCase()); // returns 'sun is red'// trim white space   --> {string}.trim()const strSpace = '   This is a string    ';
console.log(strSpace.trim())//  'this is a string'// Repeat a string  --> {string}.repeat(count)const stringRepeat = 'Hello ';
console.log(stringRepeat.repeat(5))// Returns 5 times 'Hello'
```

若要获取字符串的长度，请使用{string.length}方法。要找到一个特定的单词开始索引，你可以使用{string.indexOf()}方法，如果找到，它返回索引，如果没有找到，返回-1。如果您想在给定的索引中查找特定的字符，您必须使用{string.charAt(index)}方法。
如果你想在一个字符串中检查精确匹配的单词你可以使用{string.includes(word)}方法。此方法区分大小写。

```
// Length of a string  --> {string}.lengthconst str3 = 'Hello World';
console.log(str3.length); // The result will be 11// Finding a word inside a string --> string.indexOf({word})const largeString='How are you today ? I hope you are doing fine';
console.log(largeString.indexOf('hope'));//Returns 22 starting point// Finding a character inside a string --> {string}.charAt(index)const string = 'I love Coding in JavaScript';
console.log(string.charAt(17));   // Returns 7// Finding exact match word --> {string}.includes({word})const string = 'Hello how are you ?';
console.log(string.includes(you))// Returns true
console.log(string.includes(You))// Returns false
```

现在，为了匹配一个[正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)的目的，你可以使用{ string . match(Regular Expression)}方法。该方法对于表单验证非常有用。正则表达式可以决定过滤的数据是什么，方法将根据它提供输出。

```
//matching against a Regular Expression{string}.match(regex)const string = 'I love to travel around the World';
const refex = /[A-Z]/g;
console.log(string.match(regex)); // Returns capital words ['I' 'W']
```

**号码类型:**

数字是任何编程语言的重要组成部分。与其他编程语言不同，JavaScript 只有一种类型。对于赋给变量的每个数字，JavaScript 都将其视为双精度类型。要创建一个数字类型，你只需要把它赋给一个变量。

```
// In C++ Number declaration....int num1 = 5; // number of integer type
float num2 = 5.80  // number of floting point type// In JavaScript....const num1 = 5; // number of double type
const num2 = 5.8 // number of double type
console.log(typeof(5))// returns number
```

让我们看看 JavaScript 中的一些数字限制。最大安全整数是，要获得最大安全数，只需使用该数字。MAX_SAFE_INTEGER 方法类似地使用数字来获得最小安全数。关于`-(253 - 1)`的 MIN_SAFE_INTEGER 方法。

如果你想在 JavaScript 中找到最小的正数，只需使用 number。最小值和使用数。最大正数的 MAX_VALUE。在 JavaScript 中，任何大于 MAX_VALUE 的值都被视为无穷大。

```
// Maximum - Minimum Safe Integerconsole.log(Number.MAX_SAFE_INTEGER) // returns 9007199254740991
console.log(Number.MIN_SAFE_INTEGER) // returns -9007199254740991// Smallest - Largest Positive Numberconsole.log(Number.MIN_VALUE) // returns 5e-324 --> 5 × 10-324
console.log(Number.MAX_VALUE) // returns 1.7976931348623157e+308 --> 2^1024
```

现在让我们来探索一些 number 中的常用函数，这些函数在编码时会很方便。
首先，有 isNaN()这个确定给定值是否为数字，isNaN 导出的不是数字所以如果给定值不是数字 isNaN()为真否则为假。其次，我们有 Number.isFinite()，它当然根据名字来判断一个值是否是有限的。如果值是有限的，则返回 true，否则返回 false。第三个是 Number.isInteger()，如果给定值是整数，它也返回 true，否则返回 false。

```
// isNaN() 
console.log(isNaN('a')); // returns true
console.log(isNaN('3')); // returns false// Number.isFinite()
console.log(Number.isFinite(1 / 0)); // retuns: false
console.log(Number.isFinite(10 / 5)); // returns: true// Number.isInteger()
console.log(Number.isInteger(.6)); // returns: false
console.log(Number.isInteger(6)); // returns: true
```

**数组类型:**

最后一个主题是数组类型。作为定义，我们可以说数组是一个类似列表的对象，它收集相似类型的数据。在 JavaScript 中，你不必为了声明数组而定义数组类型，你只需用[]括号将一组数据赋给一个变量。JavaScript 以对象的形式定义数组，例如:

```
// Arrays in JavaScript
let animals = ['Dog', 'Cat', 'Cow'];
console.log(typeOf(animals)); // returns Object
```

让我们来了解一些有用的数组函数和方法。要找到数组的长度，只需使用 array.length 方法

您可以使用 array.push()在数组末尾添加一个项，类似地，使用 array.pop()从末尾移除一个项。

如果要从数组的开头移除一个项，可以使用 array.shift()并使用 array 从开头添加一个项。Unshift()方法。

要通过索引一个数字来查找数组中的一个项，只需使用 array[index],但是如果你想查找给定项在数组中的索引位置，你可以使用 array.indexOf(item)方法。

要从索引位置移除一个项目，只需使用 array.splice(index)这将返回一个项目。若要移除多个项，请在 array.splice(inex1，index2)方法中使用多个索引，这将返回一个数组。

要复制一个数组，只需使用 array.slice()方法。但是如果你想从某个位置分离一个项目，使用 array.splice(index)。

```
// Array length
const animals = ['Cat', 'Dog', 'Chicken', 'Cow'];
console.log(animals.length); // Returns 4// adding items in Array end
animals.push('goat');
console.log(animals);// returns ['Cat', 'Dog', 'Cow', 'goat'];//removing items from Array end
animals.pop();
console.log(animals);// returns ['Cat', 'Dog', 'Cow'];//adding item in Array Front
animals.unShift('sheep');
console.log(animals);// returns ['Cat', 'Dog', 'Cow', 'sheep'];//removing item in Array Front
animals.shift();
console.log(animals);// returns ['Cat', 'Dog', 'Cow'];// Finding an item by index 
console.log(animals[2]) // returns 'Cow'//finding index of an item
console.log(animals.indexOf(Cow))// returns 2//Copying an array
let animalsCopy = animals.slice();
console.log(animalsCopy); // returns ['Cat', 'Dog', 'Cow'];// Seperating specific item 
animals.splice(2);
console.log(animals); // returns ['Cow']
```

唷，时间太长了，不是吗？
我希望你从这篇文章中学到了一些 JavaScript 的基础知识。从这里开始，您必须学习对象和函数，并继续您前面的 JavaScript 之旅。
祝冒险者好运！