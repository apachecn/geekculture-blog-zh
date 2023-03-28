# 作为开发人员，为了节省时间，您应该知道的重要 JavaScript 简写第 1 部分

> 原文：<https://medium.com/geekculture/important-javascript-shorthands-you-should-know-as-a-developer-to-save-your-time-part-1-b0d1b398d982?source=collection_archive---------31----------------------->

![](img/49367dd44c2aea4bc5818f00ef135de9.png)

# 三元运算符

这是您可以使用的代替 if，else 语句，以减少 javascript 中的代码行。

if，else 的传统方式:

```
const x = 220;
let answer;

if (x > 100) {
    answer = "greater than 100";
} else {
    answer =  "less than 100";
}
```

带三元运算符

```
const answer = x > 100 ? "greater than 100" : "less than 100";
```

不仅是一个 if-else 语句，你还可以用这个三元运算符编写嵌套的 if-else 语句。

```
const answer = x > 10 ? "greater than 10" : x < 5 ? "less than 5" : "between 5 and 10";
```

# 短路评估速记

当将一个变量值赋给另一个变量时，您可能希望确保源变量不为 null、空或未定义，以便克服意外错误。为此，您可以使用传统的 if、else 语句，也可以用一行代码来编写。

传统的做法是:

```
if (variable1 !== null || variable1 !== undefined) {
     let variable2 = variable1;
}
```

速记:

```
const variable2 = variable1  || 'new';let variable1;
let variable2 = variable1  || 'bar';
console.log(variable2 === 'bar'); // prints true

variable1 = 'foo';
variable2 = variable1  || 'bar';
console.log(variable2); // prints foo
```

# 声明变量速记

如果我们想声明两个变量，同时给另一个变量赋值，我们通常这样做。

```
let x;
let y;
let z = 3;
```

但是我们可以这样做来节省时间。

```
let x, y, z=3;
```

# 对象属性速记

es6 提供了一种组织对象的好方法。如果变量名和键名相等，您可以利用简写符号。

传统方式:

```
const x = 1000, y = 2000;
const obj = { x:x, y:y };
```

用速记法:

```
const x = 1920, y = 1080;
const obj = { x, y };
```

# 隐式返回速记

return 关键字是返回一个函数的最终结果。但是带有单个语句的 arrow 函数将隐式返回值。为了减少 return 关键字的使用，您应该确保省略({})。

```
function multiplyByThree(a) {
  return a*3
}const multiplyByThree = a => (
  a * 3;
)
```

# 模板文字

为了将几个变量连接成一个字符串，我们通常使用+符号，但有时当我们在一个字符串中使用更多的变量时，我们会感到厌倦。如果你能使用 es6，那么你是非常幸运的，因为 es6 提供了一种更简单的方法来使用反勾号，并用${}将变量括起来。

```
const welcome = 'You have logged in as ' + firstName + ' ' + lastName + '.'
```

速记:

```
const welcome = `You have logged in as ${firstName} ${lastName}`;
```

# 扩展运算符速记

扩展运算符是在 es6 中引入的。它有几个让 javascript 代码更高效的用例。它也可以用来代替一些由三个点组成的数组函数。

加入和克隆阵列的传统方式。

```
// joining arrays
const even= [2, 4, 6];
const nums = [1 ,3 , 5].concat(even);

// creating a copy of arrays
const arr = [1, 2, 3, 4];
const arr2 = arr.slice()
```

使用扩展运算符。

```
// joining arrays
const even= [2, 4, 6];
const nums = [1 ,3 , 5, ...even];
console.log(nums); // [ 1 ,3 , 5, 2, 4, 6 ]

// creating a copy of arrays
const arr = [1, 2, 3, 4];
const arr2 = [...arr];
```

这里使用 spread 操作符比使用 Concat 函数有优势。你可以在任何你想把数组组合成另一个数组的地方使用 spread 操作符。

```
const odd = [1, 3, 5 ];
const nums = [2, ...odd, 4 , 6];
```

# 结论

我希望你已经掌握了一些关于 javaScript 速记的知识，并将在你进一步的编码中使用它。我将在接下来的故事中解释更多的速记。感谢您的阅读