# JavaScript 中的控制流:条件语句

> 原文：<https://medium.com/geekculture/control-flow-in-javascript-conditional-statements-acfdda94c97a?source=collection_archive---------48----------------------->

这篇文章是我在 JavaScript 系列中的控制流的一部分。在本文中，我们将讨论条件语句。

![](img/335c69ad6c3baf2593f8bee1ae9b3980.png)

Photo by [Rohan Makhecha](https://unsplash.com/@rohanmakhecha?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 什么是控制流，我们为什么需要它？

[*“在计算机科学中，* ***控制流*** *是各个语句、指令或函数调用执行/求值的顺序。*](https://en.wikipedia.org/wiki/Control_flow)

不是总是线性地执行指令，在编程中经常是，根据当前的条件，将有不止一个可能的选项可以执行。

这导致需要能够将决策分支到两个或更多选项的方法，甚至在某些情况下返回。

条件语句使程序能够根据当前条件从两个或更多可能的执行路径中选择一个。

循环使程序能够在指定的(或未指定的)时间内重复一段代码。

## 真理和谬误

在深入研究条件语句之前，让我们先了解 JavaScript 中值的“真”和“假”。

就像内存中的一位只能计算为 1 或 0(真或假)一样，JavaScript 中的每个值在布尔上下文中都可以计算为真或假。

在布尔上下文中评估为*真*的值被视为**真**。JavaScript 中的大多数值都是真实的。根据 Mozilla 开发者网络，“所有的值都是真实的，除非它们被定义为 **falsy** ”

在布尔上下文中评估为*假*的值被认为是**假**。

JavaScript 中为**false**的所有值的列表(评估为 *false* ):

*   `false`
*   `0`
*   `-0`
*   `0n`
*   `""`
*   `null`
*   `undefined`
*   `NaN`

注:有关更多信息，请参见 [MDN Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) 网页。

## 比较运算符

比较运算符位于布尔语句中，根据比较运算符两侧的条件，布尔语句的计算结果为 true 或 false。

比较运算符的类型:

*   `==`(宽松平等)
*   `!=`(对宽松平等的否定)
*   `===`(严格相等)
*   `!==`(对严格相等的否定)
*   `>`(大于)
*   `<`(小于)
*   `>=`(大于等于)
*   `<=`(小于或等于)

比较运算符的示例:

```
let x = 1;
let y = 2;
console.log(x == y); // false
console.log(x != y); // true
console.log(x === y); // false
console.log(x !== y); // true
console.log(x > y); // false
console.log(x < y); // true
console.log(x >= y); // false
console.log(x <= y); // true
```

## 逻辑运算符

逻辑运算符最常用于链接多个布尔比较语句，并根据条件返回真或假。

3 种最常见的逻辑运算符是逻辑 AND ( `&&`)、逻辑 OR ( `||`)和逻辑 NOT ( `!`)。

## 逻辑与(`&&`)

逻辑 AND ( `&&`)用于布尔语句中，仅当语句两边的计算结果都为`true`时，其计算结果为`true`。

```
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false
```

## 逻辑或(`||`)

逻辑 OR ( `||`)用于布尔语句中，只要语句的一边计算为`true`，则计算为`true`。

```
console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

## 短路评估

那么，当调用`&&`或`||`来计算一个布尔表达式时，到底发生了什么呢？

当给定一个要评估的表达式时，`&&`和`||`将评估表达式左侧的`true`或`false`。在此之后，根据运算符是逻辑 AND 还是 or，要么返回表达式左侧的*原*，要么返回右侧的*原*。这被称为`short-circuit evaluation`

`&&`返回第一个虚假值/评估。如果所有表达式的计算结果都为 the，则返回最右边的值。

```
// 0 (0 is falsy, everything else is ignored)
console.log(0 && 1 && 2); // 0

// 0 (1 is truthy, so we look at the next value which is 0,
// since 0 is falsy, it is returned, and everything else
// is skipped)
console.log(1 && 0 && 2); // 0

// 0 (1 is truthy, and so is 2, so the next value to the right is 0, which is falsy, it is therefore returned)
console.log(1 && 2 && 0); // 0

// 3 (everything is truthy, return right most item)
console.log(1 && 2 && 3); // 3

// true, (both left and right sides of && first evaluate to
// true, since true on both sides of &&, return true
// (nothing is falsy))
console.log(1 < 2 && 4 > 3); // true
```

`||`返回第一个真值/评估。如果表达式的计算结果为 falsy，则返回最右边的值。

```
// 1 (0 is falsy, so 1 is evaluated next,
// since 1 is truthy, it is returned and everything else
// is ignored)
console.log(0 || 1 || 2); // 1

// 1 (1 is truthy, so it is returned,
// everything else is ignored)
console.log(1 || 0 || 2); // 1

// 1 (1 is truthy, and so is 2, but since 1 was the 1st
// truthy value, it is returned, and everything is skipped)
console.log(1 || 2 || 0); // 1

// 3 (0 and undefined are both falsy, and 3 is truthy, so
// 3 is returned)
console.log(0 || undefined || 3); // 3

// undefined (since 0, false, and undefined are falsy,
// the right-most falsy value is returned)
console.log(0 || false || undefined); // undefined
```

**注意**:记住，短路评估的工作方式是首先评估使用比较运算符的布尔表达式，然后短路评估开始并接管。因此，任何带有比较运算符的结果都将是`true`或`false`，这就是逻辑 and 或 or 将返回的结果。

```
// returns true (1 < 2 evaluates to true,
// so the value of true is returned)
console.log(1 < 2 || 0); // true

// returns 0 (1 > 2 evaluates to false, so || returns
// the right hand side by default, which is 0)
console.log(1 > 2 || 0); // 0
```

## 逻辑非(`!`)

逻辑非(`!`)反转它前面的操作数的真或假。基本上，如果某个值为真，逻辑 NOT 会将它变为假，反之亦然。

```
console.log(!true); // false
console.log(!false); // true
```

## if 语句:

`if`语句评估一个条件(括号中的内容)。当条件被评估为`truthy`时，`if`语句将运行花括号内的代码块。

如果条件被评估为`falsy`，则`if`语句及其花括号内的内容不会被评估，JavaScript 会继续处理代码中位于`if`语句右花括号之后的下一条语句..

```
// The stuff inside the parenthesis is the condition that
// is used to determine if the contents inside the
// curly braces {} will run or not.
// The condition will either evaluate to be truthy or falsy
if (true) {
  console.log("the if statement has run");
}

if (false) {
  // this code is skipped since the condition in the if
  // statement is false
  console.log("this code will not run");
}
```

## if…else 语句:

`if...else`语句的`else`部分是对 if 语句的补充。

基本上，当 if 语句评估为`false`时，属于`if`语句一部分的代码块将被跳过，而将运行`else`块中的代码。

由于`else`语句没有要评估的条件，只要它上面的`if`和`else if`语句全部失败(即它们的条件评估为`false`)，它就会一直运行；

通知；注意

```
if (true) {
  // code in if loop will run since it evaluates to true
  console.log("this code will run");
} else {
  // this code will not run when the if statement runs
  // it will only run when the condition in the if
  // statement evaluates to false
  console.log("this code will not run");
}

if (false) {
  // code inside if statement will not run as the condition
  // evaluates to false
  console.log("this code will not run");
} else {
  // code inside else statement will run since the
  // the condition in the if statement is false
  console.log("this code will run");
}
```

## else if 语句:

`else if`语句位于`if`和`else`语句之间。您可以在`if`和`else`语句之间插入任意多的`else if`语句。

每个`else if`语句的代码块将仅在`else if`语句内的条件评估为`true`时运行，并且上述任何`if`或`else if`语句也评估为`false`时运行。

当`else if`语句运行时，当前`else if`语句下的任何附加`else if`和`else`语句都不会运行。

```
const x = 1;
const y = 2;
const z = 3;

if (x < 1) {
  // The condition in this if statement is false,
  // so this if statement will not run
  console.log(x, "< 1");
} else if (x === y) {
  // The condition in this else if evaluates to false
  // so this else if statement will not run
  console.log(x + "=" + y);
} else if (x === 1) {
  // This is the first condition that evaluates to true
  // it will run
  console.log("x = 1");
} else if (y === 2) {
  // while the condition in this else if statement is true
  // the else if statement above was also true and was
  // evaluated first. Since there was already a statement
  // which evaluated to true and ran, no other statements
  // below it will run, including this else if statement.
  console.log(
    "this code will not run because the else if block above ran first"
  );
} else {
  console.log(
    "this code will not run because a previous else if statement executed successfully"
  );
}
```

## 开关语句:

Switch 语句的工作方式与 if 循环略有不同。switch 语句接受一个表达式，然后在它的一个`case`中寻找该表达式的值，而不是评估一个条件为真或假。

在 switch 语句中，可以有任意多的`case`条件。

当一个 switch 语句识别出一个匹配的`case`时，switch 语句将运行该`case`中的所有内容以及它下面的任何其他代码，包括其他`case`条件。

如果没有识别出匹配的`case`，则运行默认情况(假设它是开关表达式中的最后一个条件)。

为了避免运行多个案例，最好在每个`case`语句的最后一行添加一个`break`语句。这将导致 switch 表达式在运行到`break`语句时退出。

下面是用于编写 switch 语句的语法，由 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) 提供:

```
switch (expression) {
  case value1:
    //Statements executed when the
    //result of expression matches value1
    [break;]
  case value2:
    //Statements executed when the
    //result of expression matches value2
    [break;]
  ... // you can have as many cases as you want
  case valueN:
    //Statements executed when the
    //result of expression matches valueN
    [break;]
  [default:
    //Statements executed when none of
    //the values match the value of the expression
    [break;]]
}
```

这里有一个 switch 语句的例子，也是由 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) 提供的。根据`expr`的值，可能会发生一些不同的事情。

如果`expr`是`Oranges`:

*   "橙子每磅 0.59 美元。"将打印输出到控制台。
*   break 语句将触发并阻止当前`case`之下的任何内容执行。

如果`expr`为`Apples`:

*   "苹果每磅 0.32 美元。"将打印输出到控制台。
*   break 语句将触发并阻止当前`case`之下的任何其他内容执行。

如果`expr`为`Bananas`:

*   "香蕉每磅 0.48 美元。"将打印输出到控制台。
*   break 语句将触发并阻止当前`case`之下的任何其他内容执行。

如果`expr`是`Cherries`:

*   "樱桃每磅 3 美元。"将打印输出到控制台。
*   break 语句将触发并阻止当前`case`之下的任何东西执行。

如果`expr`是`Mangoes`:

*   `Mangoes`中的 case 将会运行，这个 case 中实际上没有任何东西，但是也没有 break 语句，所以`Mangoes` case 下面的所有东西也将会运行(本例中的`Papayas` case)

如果`expr`是`Papayas`:

*   "芒果和木瓜每磅 2.79 美元."将打印输出到控制台。
*   break 语句将触发并阻止当前`case`以下的任何内容执行。

如果`expr`不是这些:

*   将运行默认情况
*   抱歉，我们没有“+ expr +”了。将在控制台中运行，将`expr`替换为您设置的值。

```
const expr = "Apples";

switch (expr) {
  case "Oranges":
    console.log("Oranges are $0.59 a pound.");
    break;
  case "Apples":
    console.log("Apples are $0.32 a pound.");
    break;
  case "Bananas":
    console.log("Bananas are $0.48 a pound.");
    break;
  case "Cherries":
    console.log("Cherries are $3.00 a pound.");
    break;
  case "Mangoes":
  case "Papayas":
    console.log("Mangoes and papayas are $2.79 a pound.");
    break;
  default:
    console.log("Sorry, we are out of " + expr + ".");
}

console.log("Is there anything else you'd like?");
```

为了查看上述所有情况下会发生什么，我修改了上面的代码，以遍历包含所有 case 选项的数组。

```
const expr = [
  "Oranges",
  "Apples",
  "Bananas",
  "Cherries",
  "Mangoes",
  "Papayas",
  "Steak",
];

for (const item of expr) {
  switch (item) {
    case "Oranges":
      console.log("Printing results of 'Oranges' case:");
      console.log("Oranges are $0.59 a pound.");
      break;
    case "Apples":
      console.log("Printing results of 'Apples' case:");
      console.log("Apples are $0.32 a pound.");
      break;
    case "Bananas":
      console.log("Printing results of 'Bananas' case:");
      console.log("Bananas are $0.48 a pound.");
      break;
    case "Cherries":
      console.log("Printing results of 'Cherries' case:");
      console.log("Cherries are $3.00 a pound.");
      break;
    case "Mangoes":
      console.log("Printing results of 'Mangoes' case:");
    case "Papayas":
      console.log("Printing results of 'Papayas' case:");
      console.log("Mangoes and papayas are $2.79 a pound.");
      break;
    default:
      console.log("Printing results of 'default' case:");
      console.log("Sorry, we are out of " + item + ".");
  }
}

console.log("Is there anything else you'd like?");
```

以下是打印到控制台的内容:

```
Printing results of 'Oranges' case:
Oranges are $0.59 a pound.
Printing results of 'Apples' case:
Apples are $0.32 a pound.
Printing results of 'Bananas' case:
Bananas are $0.48 a pound.
Printing results of 'Cherries' case:
Cherries are $3.00 a pound.
Printing results of 'Mangoes' case:
Printing results of 'Papayas' case:
Mangoes and papayas are $2.79 a pound.
Printing results of 'Papayas' case:
Mangoes and papayas are $2.79 a pound.
Printing results of 'default' case:
Sorry, we are out of Steak.
Is there anything else you'd like?
```

## 条件运算符

条件运算符本质上是执行 if…else 循环的捷径。您还将看到称为*三元*运算符的条件运算符。

您不需要编写 if else 循环，而是编写用于评估真值(或假值)的条件，然后添加一个问号，后跟一个条件为真时要运行的表达式、一个冒号(:)，以及另一个条件评估为假时要运行的表达式。

下面是条件运算符使用的语法:

```
(condition) ? expressionIfTrue : expressionIfFalse
```

基本上，如果括号(问号左边)中的条件计算结果为 true，则返回冒号左边的表达式。如果条件的计算结果为 false，则返回冒号右侧的表达式。

```
let x = 1;
let y = 2;

let a = true ? x : y;
console.log(a); // 1

let b = false ? x : y;
console.log(b); // 2

// Logs "Hi" to the console
let c = 30 < 60 ? console.log("Hi") : console.log("Goodbye");

// Logs "Goodbye" to the console
let d = 30 > 60 ? console.log("Hi") : console.log("Goodbye");
```

## 资源:

*   [MDN —控制流程和错误处理](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
*   [MDN — Truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)
*   [MDN — Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
*   [MDN —逻辑非(！)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_NOT)
*   [MDN —开关](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)
*   [MDN —条件(三元)运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
*   [维基百科—控制流](https://en.wikipedia.org/wiki/Control_flow)
*   [维基百科—有条件(计算机编程)](https://en.wikipedia.org/wiki/Conditional_(computer_programming))
*   [雄辩的 JavaScript，第三版:第二章，程序结构](https://eloquentjavascript.net/02_program_structure.html)
*   [Javascript。信息:逻辑运算符](https://javascript.info/logical-operators)
*   [Javascript。信息:条件分支:如果，'？'](https://javascript.info/ifelse)