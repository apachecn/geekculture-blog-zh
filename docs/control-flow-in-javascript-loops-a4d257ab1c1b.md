# JavaScript 中的控制流:循环

> 原文：<https://medium.com/geekculture/control-flow-in-javascript-loops-a4d257ab1c1b?source=collection_archive---------30----------------------->

这篇文章是我在 JavaScript 系列中的控制流的一部分。在本文中，我们将讨论循环。

![](img/a169a872471a75684dfe57b1c3652a35.png)

Photo by [Rohan Makhecha](https://unsplash.com/@rohanmakhecha?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 为什么我们需要循环？

在代码中，您经常会发现自己处于需要重复执行一项或多项特定任务的情况。

假设您想在控制台上打印数字 1 到 10。实现这一点的一种方法是:

```
console.log(1);
console.log(2);
console.log(3);
console.log(4);
console.log(5);
console.log(6);
console.log(7);
console.log(8);
console.log(9);
console.log(10);
```

好吧，这只有 10 行代码，你可能会说这没那么糟糕。如果不是打印数字 1 到 10，而是要求您打印数字 1 到 1000，会怎么样呢！你真的要打出 1000 条`console.log()`语句吗？我们可以实现以下循环，而不是写 1000 行，例如:

```
// Print out the numbers 1 through 1000 to the console
for (let i = 0; i < 1000; i++) {
  console.log(i);
}
```

循环使程序能够在指定的(或未指定的)时间内重复一段代码。

# 循环基础

所有 3 个标准循环(for、while 和 do-while)需要 3 样东西才能正确运行:

1.  迭代器/初始条件。
2.  评估为 true 或 false 的条件，以确定循环是否应该运行。通常，这个条件与迭代器/初始条件相关联。
3.  一种增加迭代器/初始条件的方法。

## 对于循环:

在所有 3 个标准循环中，`for`循环是最常用的循环。

下面是语法:

```
for (iterator; condition; incrementIterator) {
  // Code in for block goes here
  // This code will only execute if the condition
  // evaluates to true
}
```

让我们来看一个 for 循环的例子，并逐步说明发生了什么:

```
// Initialize an array
let myArray = ["a", "b", "c", "d", "e"];

// Loop through the array and print each element
for (let i = 0; i < myArray.length; i++) {
  console.log(myArray[i]);
}

// The above loop prints out
// a
// b
// c
// d
// e

// This console.log() will run after the for loop has
// completed printing out all of the elements in the
// array
console.log("For loop ended");
```

1.  当 for 循环第一次运行时，迭代器被设置为 0。
2.  然后检查条件，因为 0 小于 myArray.length (5)，所以条件评估为`true`。
3.  由于条件评估为`true`，那么 for 循环中的代码运行一次，数组中的第一个元素被打印到控制台。
4.  循环中的代码执行一次后，迭代器由`i++`从 0 递增到 1。
5.  此后，再次检查条件，因为 1 小于数组的长度，所以再次运行 for 循环中的代码，并将数组的第二个值打印到控制台。
6.  代码第二次运行后，迭代器再次加 1，所以现在它的值是 2。
7.  重复检查条件、运行代码和递增迭代器的循环，直到迭代器递增到数组长度 5。此时，条件不再成立，因为 5 < 5 是`false`。这导致 for 循环终止并移动到下一组代码`console.log("For loop ended")`；

## while 循环:

与 for 循环不同，while 循环在 while 循环声明之外初始化迭代器。与 for 循环不同的是，迭代器的递增不会自动发生，而是需要在 while 循环代码块中明确声明，否则迭代器不会递增，while 循环将永远循环下去。这叫做`infinite loop condition`。这是应该避免的，因为一旦你进入了一个无限循环，你就不能从代码中摆脱它，你将不得不手动关闭或退出你的程序。

以下是 while 循环的语法:

```
let iterator = someValue;
while (condition) {
  // Code goes here

  // If we don't increment the iterator here, our
  // loop will probably keep going to infinity
  iterator++;
}
```

**注意**:从技术上讲，你不需要迭代器来使用 while(或 do…while)循环。然而，如果你不使用迭代器，你需要有一些其他的方法来确保你的 while 循环中的条件最终评估为 false，否则你将结束一个无限循环。除了使用迭代器，您还可以在循环中使用 if 条件来检查某个标志是否为某个值，如果是，则在 while 循环中将条件改为 false。

```
// Initialize variable to be printed (and decremented) in loop
let n = 5;

// Initialize flag to be used in while loop evaluation
let flag = true;

// Loop while flag evaluates to true
while (flag) {
  // Log values to console
  console.log("Flag is true");
  console.log(n);
  n--; // Decrement n

  // Condition that flips flag to false and ends
  // execution of while loop
  if (n < -5) {
    console.log("Flag is false");
    flag = false;
  }
}

/* CODE OUTPUT:
Flag is true
5
Flag is true
4
Flag is true
3
Flag is true
2
Flag is true
1
Flag is true
0
Flag is true
-1
Flag is true
-2
Flag is true
-3
Flag is true
-4
Flag is true
-5
Flag is false
*/
```

现在让我们来看一个例子，在这个例子中，我们循环遍历一个数组并打印它的所有值:

```
// Initialize an array
let myArray = ["a", "b", "c", "d", "e"];

// Set an iterator with an initial value
// for the while loop
let i = 0;

// Loop through the array and print each element
while (i < myArray.length) {
  // Log the current element in the array to the console
  console.log(myArray[i]);

  // Increment the iterator
  i++;
}

// The above loop prints out
// a
// b
// c
// d
// e

// This console.log() will run after the loop has
// completed printing out all of the elements in the
// array
console.log("while loop ended");
```

1.  在上面的例子中，我们在 while 循环之外初始化迭代器，并将其值设置为 0。
2.  while 循环检查条件为`i < myArray.length`，由于`i`当前为 0，循环将运行并打印数组中的第一个元素，同时递增循环外声明的迭代器。
3.  然后，在内部代码运行之前，重复检查 while 循环的条件。
4.  一旦 while 循环内的迭代器增加到与数组长度相同的值 5，while 循环上的条件将不再是`true`，while 循环将退出并移动到下一组指令`console.log("while loop ended")`。

## do…while 循环:

Do while 循环与 while 循环非常相似，只是条件检查发生在循环内容执行之后。这确保了即使 while 循环内的条件将立即评估为`false`，由于条件评估为`false`，循环内的内容将在循环退出之前运行一次。

`do...while`循环的语法:

```
// Initialize an iterator which will be used to control
// how many times the loop will run.
let iterator = someValue;

// Run the code inside the do code block
do {
  // Code goes here

  // If we don't increment the iterator here, our
  // loop will probably keep going to infinity
  iterator++;

  // check the condition evaluates to true
  // before going back and running the code again
  // inside the do loop
} while (condition);
```

do…while 循环示例:

```
// Initialize an array
let myArray = ["a", "b", "c", "d", "e"];

// Set an iterator with an initial value
// for the do...while loop
let i = 0;

// Loop through the array and print each element
do {
  // Log the current element in the array to the console
  console.log(myArray[i]);

  // Increment the iterator
  i++;
} while (i < myArray.length);

// The above loop prints out
// a
// b
// c
// d
// e

// This console.log() will run after the loop has
// completed printing out all of the elements in the
// array
console.log("do...while loop ended");
```

1.  这里，迭代器也在循环外部声明，并初始化为初始值 0。
2.  运行`do...while`循环中的代码，迭代器递增 1。
3.  然后检查 while 循环中的条件。因为 1 小于数组的长度，所以循环中 do 部分的代码将再次运行。
4.  这种检查条件并在 do 块中运行代码的循环会一直重复，直到 while 循环中的条件不再为真。此时，`do...while`循环退出，下一段代码运行，即`console.log("do...while loop ended")`。

# 跳过迭代和跳出循环:

## 休息:

JavaScript 中的 break 语句用于循环内部，以提前退出循环。这些通常出现在`if`语句中，用于帮助控制循环。

`break`语句的一个特别有用的用途是打破一个无限的 while 循环。

如果在嵌套的(循环中的循环)循环中发现了`break`语句，那么`break`语句只强制 JavaScript 从包含 break 语句的最内层循环中跳出。

使用 break 语句的示例:

```
for (let i = 0; i < 10; i++) {
  console.log(i);
  if (i === 3) {
    break;
  }
}

console.log("printing outside for loop");

/*
Output of code above
0
1
2
3
printing outside for loop
*/

for (let i = 0; i < 5; i++) {
  console.log("Printing i:", i);
  for (let j = 0; j < 5; j++) {
    if (j > 3) {
      break;
    }
    console.log("Printing j:", j);
  }
}

/*
Output of Nested For Loop:
Printing i: 0
Printing j: 0
Printing j: 1
Printing j: 2
Printing j: 3
Printing i: 1
Printing j: 0
Printing j: 1
Printing j: 2
Printing j: 3
Printing i: 2
Printing j: 0
Printing j: 1
Printing j: 2
Printing j: 3
Printing i: 3
Printing j: 0
Printing j: 1
Printing j: 2
Printing j: 3
Printing i: 4
Printing j: 0
Printing j: 1
Printing j: 2
Printing j: 3
*/

// You can also use the break statement to break out of an infinite while loop

let counter = 0;
while (true) {
  console.log(counter);
  counter++;
  if (counter > 5) {
    break;
  }
}

/*
Output of while loop:
0
1
2
3
4
5
*/
```

## 继续:

`continue`语句的工作方式类似于`break`语句，只是`continue`并没有完全跳出包含`continue`语句的循环，而是强制当前循环开始下一次迭代，同时跳过`continue`语句下面的任何附加语句。

更具体地说，当执行`continue`语句时，根据语句所在的循环类型，有两种可能性:

*   对于`while`循环，`continue`强制循环进行下一次迭代。
*   对于`for`循环，`continue`强制循环更新当前迭代器，然后继续下一次迭代。

同样类似于`break`语句，`continue`只作用于包含`continue`语句的最内层循环。

```
for (let i = 0; i < 5; i++) {
  if (i === 3) {
    continue;
  }
  console.log(i);
}

console.log("printing outside for loop");

/*
Notice how the value of 3 is not printed. This is because
the if statement triggers and the continue causes
the console.log(i) to get skipped and the next iteration
proceeds.
Output of code above:
0
1
2
4
printing outside for loop
*/

for (let i = 0; i < 5; i++) {
  console.log("Printing i:", i);
  for (let j = 0; j < 5; j++) {
    if (j === 2) {
      continue;
    }
    console.log("Printing j:", j);
  }
}

/*
NOTE: Notice how the number 2 is not being printed
inside the nested for loop. This is because of the
continue statement.
Output of Nested For Loop:
Printing i: 0
Printing j: 0
Printing j: 1
Printing j: 3
Printing j: 4
Printing i: 1
Printing j: 0
Printing j: 1
Printing j: 3
Printing j: 4
Printing i: 2
Printing j: 0
Printing j: 1
Printing j: 3
Printing j: 4
Printing i: 3
Printing j: 0
Printing j: 1
Printing j: 3
Printing j: 4
Printing i: 4
Printing j: 0
Printing j: 1
Printing j: 3
Printing j: 4
*/
```

# 用 for…of 和 for…in 循环遍历 JS 中的 Iterables 和 Objects:

## 对于…循环:

`for...of`循环是编写 for 循环的一种快捷方式，用于迭代 iterable 对象中的所有元素。`strings`、`arrays`、`maps`和`sets`是 JavaScript 中可迭代对象的例子。使用`for...of`也可以访问类似数组的对象中的元素，如`NodeList`。

当使用一个`for...of`循环时，在循环的条件语句中声明的迭代器采用被评估的 iterable 中当前元素的值。

```
let myArray = ["a", "b", "c", "d", "e"];

for (let letter of myArray) {
  console.log(letter);
}

/*
Output from for...of array
a
b
c
d
e
*/
```

## 对于…在循环中:

`for...in`循环遍历对象中的属性，特别是它们的键。

```
let myObject = {
  firstName: "John",
  lastName: "Doe",
  age: 50,
};

for (let property in myObject) {
  console.log(`${property}: ${myObject[property]}`);
}

/*
Output from the for...in loop
firstName: John
lastName: Doe
age: 50
*/
```

**注意:**虽然可以使用`for...in`循环来迭代数组，但是请只使用`for...in`循环来迭代对象属性。`for...in`循环不一定按照特定的顺序遍历数组。

# 参考

*   [MDN —用于](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)
*   [MDN — while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while)
*   [MDN — do…while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/do...while)
*   [MDN —继续](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue)
*   [MDN — break](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break)
*   [MDN——内置可重复项](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#built-in_iterables)
*   [MDN —用于](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)的……
*   [MDN —在](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)中用于……