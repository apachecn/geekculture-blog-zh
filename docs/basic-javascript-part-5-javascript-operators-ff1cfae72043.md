# 基础 JavaScript 第 5 部分:JavaScript 操作符

> 原文：<https://medium.com/geekculture/basic-javascript-part-5-javascript-operators-ff1cfae72043?source=collection_archive---------10----------------------->

![](img/faa0cc2892fa9209fc42e9c63e95192f.png)

你好，各位朋友好，这次我们将继续 JavaScript 教程。之前我们讨论了 JavaScript 变量。运算符是编程时必须理解的基本代码。因为如果我们做一个应用，我们肯定会在其中找到数理逻辑。

在继续之前，请先学习前面的教程:

[基础 JavaScript 第 1 部分:什么是 JavaScript？](https://temanngoding.com/en/basic-javascript-part-1-what-is-javascript/)

[基础 JavaScript 第二部分:第一段代码和注释](https://temanngoding.com/en/basic-javascript-part-2-first-code-and-comments/)

[基础 JavaScript 第三部分:变量](https://temanngoding.com/en/basic-javascript-part-3-variables/)

[基础 JavaScript 第 4 部分:JavaScript 数据类型](https://temanngoding.com/en/basic-javascript-part-4-javascript-data-types/)

# 什么是运算符？

运算符是一种符号，用于执行数学、理性或逻辑等运算以给出某个结果。

运算符有几种类型，介于:

1.  赋值运算符
2.  比较运算符
3.  逻辑算子
4.  按位运算符

# 赋值运算符

该运算符用于为变量赋值。基本上这个操作符给出了一个等号(=)。这里是初始化变量值的符号。下面我举个例子。

```
a = b;
```

上面的代码初始化了变量 a 中 b 的值。

赋值操作符在给变量赋值时还有另外一个好处。例如

```
var assignment = 1;
assignment += 1;
```

上面的代码示例与

```
assignment = assignment + 1;
```

在上面的例子中，我们可以通过加 1 来给一个变量赋值。此示例与 increment.ac 方法相同 increment 方法通常用于循环过程，而赋值运算符用于更复杂的算术运算。

这种方法可用于算术运算符，如减、加、乘、除等。我再举一个例子:

```
let x = 10;
let y = 5;x += y; // same -> x = x + y;
x -= y; // same -> x = x - y;
x *= y; // same -> x = x * y;
x /= y; // same -> x = x / y;
x %= y; // same -> x = x % y;
```

# 比较运算符

比较运算符是作为编程基本逻辑的一个值的比较运算符。

下面我将描述一个比较运算符，它对于计算和比较两个值很有用。

**operators function**= =比较两个值是否相同(不相同)。！=比较两个值不相等(不相同)。= = =比较两个值是否相同。！= =比较这两个值并不相同。>比较两个值，看第一个值是否大于第二个值。>=比较两个值，看第一个值是否大于或等于第二个值。<比较两个值，看第一个值是否小于第二个值。<=比较两个值，看第一个值是否小于或等于第二个值。

在上面的运算符中，我们已经知道了比较运算符类型的功能。我们来举个例子:

```
let a = 14;
let b = 16;
console.log(a < b);
console.log(a > b);/* output
true
false
*/
```

当我们创建一个程序时，我们会发现运算符“相等”(==)和“相同”(===)，我们将在下面讨论它们的区别。

当我们有两个值相同的变量时，例如第一个字符串“5”和数字 5。这是同一个数字，但它们并不完全相同。

根据上面的陈述，值“5”具有不同的数据类型。如果我们只想从相同的值进行比较，我们可以使用==如果我们想通过数据类型进行比较，我们可以使用===。下面我举个例子:

```
const aString = '5';
const aNumber = 5console.log(aString == aNumber) // true
console.log(aString === aNumber) // false
```

# 逻辑算子

逻辑运算符是确定“布尔”值的运算符，只取真和假。通过逻辑运算符，我们可以在指定逻辑时使用两个或多个布尔值。

在 JavaScript 中，我们有三个特殊字符用于逻辑操作符。以下是运算符及其功能:

**运算符说明** & &运算符丹(and)。如果满足所有条件，逻辑将返回 true(真)。| |运算符 or(或)。如果满足其中一个条件，逻辑将返回 true(真)。！操作员不在(不在)。用于逆转一种状态。

让我们直接看例子:

```
var x = 20;
  var y = 19; var aTrue = x > y;
  var aFalse = x < y; //operator && (and)
 var resultAnd = aTrue && aFalse;
console.log(resultAnd); //false //operator && (or)
var resultOr = aTrue || aFalse;
console.log(resultOr); //true // operator ! (not)
 var resultNot = !aTrue;
console.log(resultNot); //false
```

布尔逻辑的功能是编程逻辑的关键，因为布尔可以控制程序的流程。

我们将在控制程序流程的 if/else 语句中讨论这个函数。

# 按位运算符

按位运算符是用于基于位(二元)运算的运算符。以下是按位运算符的名称和符号:

javaand&or|xor^negation/opposite~left 中的名称符号右移左移(无符号)

该运算符适用于数据类型`int`、`long`、`short`、`char`、dan `byte`。

下面我举个例子:

```
var x = 4;
        var y = 3; // bitwise operator and
        var result = x & y;
        console.log(result); // bitwise operator or
        var result = x | y;
        console.log(result); // bitwise operator xor
        var result = x ^ y;
        console.log(result); // nagasi operator 
        var result = ~x;
        console.log(result);

        // bitwise operator right shift >>
        var result = x >> y;
        console.log(result); // bitwise operator right shift <<
        var result = x << y;
        console.log(result); // bitwise operator right shift (unsigned) >>>
        var result = x >>> y;
        console.log(result);
```

所以这个教程我可以给，希望它会有用。

***谢了。***