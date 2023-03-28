# JS 中的数据结构:数组

> 原文：<https://medium.com/geekculture/data-structures-in-js-arrays-2347380696a0?source=collection_archive---------25----------------------->

本系列博客将讨论 JavaScript 环境中的各种数据结构。数据结构是组织数据以解决不同类型问题的不同方式。

![](img/9d30b4da45227ba4171131e5d701c3c1.png)

Photo by [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

# 什么是数组？：

正式来说，数组是“类似列表的对象”。数组是一种数据结构，它存储了一个按**排序的**值集合。

## 数组的属性:

*   该数组保存值的集合
*   这允许我们组织多个值，并将它们赋给一个变量
*   存储在数组中的值是有序的，这意味着每一项都与特定的索引相关联
*   JS 中的数组索引从 0 开始
*   该数组可以保存任何类型的值(数字、字符串、布尔值、NaN、未定义、其他数组、对象……)
*   在 JS 中，数组可以同时保存任何数据类型的组合
*   数组数据结构存储在一个连续的内存块中
*   如果需要添加数组中的元素，那么可能需要将整个数组移动到内存中的另一个位置，以保留扩展后的数组的连续内存块
*   数组是可变的，这意味着它们的值可以在以后被访问和更改(即使它们是使用`const`关键字定义的)

包含多种数据类型的数组示例:

```
let myArray = [
  1,
  2,
  "water",
  true,
  NaN,
  { name: "Bob" },
  0,
  undefined,
  [100, 200, 300],
];
```

在用关键字`const`声明后，数组的值被访问和改变的例子。这证明了可变性:

```
const myArray = [1, 2, 3, 4, 5];
console.log(myArray); // [ 1, 2, 3, 4, 5 ]

myArray[0] = 100;
console.log(myArray); // [ 100, 2, 3, 4, 5 ]
```

# 如何创建阵列:

创建数组有两种常见的方法，使用括号符号和调用`Array()`构造函数。

**注意:**数组中的每一项都被称为一个“元素”。

## 使用方括号创建数组文字:

这是创建数组最常见的方式。要创建一个使用方括号的数组，定义一个变量，在赋值操作符的右边，在方括号之间添加所有您希望保存在数组中的值，用逗号分隔。

使用方括号创建数组的示例:

```
let myArray = [1, 2, 3, 4, 5];
let myArray2 = ["Hi", "my", "name", "is", "Bob"];
let myArray3 = [1, false, "word"];

let favoriteNum = 9;
let myArray4 = [1, 2, 3, favoriteNum];
```

**注意:**如果“数组文字”这个词让您措手不及，那么请看一下我在参考资料一节中关于文字和数组文字的 MDN 参考资料。从实用和函数的角度来看，数组文字和数组是一回事。

## 使用 new 关键字和 Array()构造函数创建数组:

要使用`Array()`构造函数创建一个数组，设置一个等于`new Array()`的变量，并在`Array()`构造函数的括号内包含要存储在数组中的值列表。

使用`Array()`构造函数方法创建数组的例子:

```
let myArray = new Array(1, 2, 3, 4, 5);
```

# 使用 length 属性获取数组的长度

为了找出数组的长度，使用点标记法调用数组的`length`属性。这将返回数组的长度，即数组中元素的数量。

```
// Create an array
let myArray = ["a", "b", "c", "d"];

// Create a myArrayLength variable and assign it
// the value equal to the length of myArray
let myArrayLength = myArray.length;
console.log(myArrayLength); // 4
```

# 如何访问数组中的元素:

为了访问数组中的元素，我们在数组名称的末尾添加一组方括号，并在方括号内添加我们要访问的数组元素的索引值。请记住，在 JS 中，数组索引从 0 开始，因此数组中的第一个元素将位于第 0 个索引处。

JS 中访问数组元素的工作方式示例:

```
// Define an array literal using bracket notation
let myArray = ["A", "B", "C"];

// access the individual elements in the array and log them
// to the console
console.log(myArray[0]); // "A"
console.log(myArray[1]); // "B"
console.log(myArray[2]); // "C"

// undefined (there isn't a 4th element in the array)
console.log(myArray[3]);
```

## 访问多维数组

多维数组是嵌套在其他数组中的数组。最常见的多维数组是 2D 数组，它是包含一个或多个数组作为元素的数组。您可以拥有任意数量的嵌套数组(例如 3D，4D…)。

要访问多维数组，请使用与数组的“维数”一样多的括号。

```
let myArray = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];

// log the 2nd sub-array in myArray
console.log(myArray[1]); // [4, 5, 6]

// take the 0th element in myArray ([1, 2, 3])
// and access the 0th element in that sub-array (1)
console.log(myArray[0][0]); // 1

// take the 3rd element in myArray ([7, 8, 9])
// and access the 3rd element in that sub-array (9)
console.log(myArray[2][2]); // 9
```

# 如何重新分配数组中的元素:

为了重新分配或修改数组中的元素，我们访问数组中所需元素的当前值，并使用赋值操作符来更改其值。

为了改变数组中元素的值，首先需要知道它的索引是什么。请记住，数组中的第一个元素位于索引 0 处，因为 JavaScript 使用基于 0 的索引系统。

```
// Define an array literal using bracket notation
let myArray = ["A", "B", "C"];
console.log(myArray); // ["A", "B", "C"]

// Access the 1st element of the array
// and reassign it to a new value
myArray[0] = 1;
console.log(myArray); // [1, "B", "C"]

// Access the 2nd element of the array
// and reassign it to a new value
myArray[1] = 2;
console.log(myArray); // [1, 2, "C"]

// Access the 3rd element of the array
// and reassign it to a new value
myArray[2] = 3;
console.log(myArray); // [1, 2, 3]
```

# 在数组末尾添加和移除元素:

根据元素在数组中的位置，可以使用不同的策略和方法(函数)来添加或删除数组中的元素。

**注意:**从计算的角度来看，在数组的末尾删除或添加元素是最简单和最不费力的。在数组前面移除或添加元素花费的时间最多，因为数组中所有元素的索引都需要更改，以考虑新元素的移除或添加。在从数组后面添加和删除元素的情况下，数组中的其他元素都不需要改变它们的索引。

## 使用方括号追加到数组末尾:

您可以使用重新分配过程实际添加到数组的末尾。

**注意:**强烈建议使用。push()方法代替这种方式将元素追加到数组的末尾。

```
// Define an array literal using bracket notation
let myArray = ["A", "B", "C"];
console.log(myArray); // ["A", "B", "C"]

myArray[3] = "D";
console.log(myArray); // ["A", "B", "C", "D"]

myArray[5] = "E";
console.log(myArray);
// [ 'A', 'B', 'C', 'D', <1 empty item>, 'E' ] (from node)
// ["A", "B", "C", empty × 2, "E"] (from web console)
```

## 。push()将元素追加到数组末尾:

使用`push()`方法将一个或多个元素追加到数组的末尾。

`push()`方法也将返回数组的新长度。

```
// Define an array literal using bracket notation
let myArray = ["A", "B", "C"];
console.log(myArray); // ["A", "B", "C"]

// Append the "D" string to the end of the array
myArray.push("D");
console.log(myArray); // ["A", "B", "C", "D"]

// Append the "E" string to the end of the array
myArray.push("E");
console.log(myArray); // ["A", "B", "C", "D", "E"]

// Append multiple values to the end of the array
myArray.push("X", "Y", "Z");
// ['A', 'B', 'C','D', 'E', 'X','Y', 'Z']
console.log(myArray);

// console.log the return value of the push() method
// (the length of the new array)
console.log(myArray.push(1)); // 9
console.log(myArray.push(2)); // 10
console.log(myArray.push(3)); // 11
console.log(myArray.push(4, 5, 6)); // 14
```

## 。pop()从数组末尾移除元素:

`pop()`方法移除数组中的最后一个**元素，并返回被移除元素的值。**

与允许在数组末尾添加多个元素的`push()`方法不同，使用`push()`方法只能删除一个元素(最后一个)。

```
// Define an array literal using bracket notation
let myArray = [1, 2, 3, 4, 5];
console.log(myArray); // [1, 2, 3, 4, 5]

// Remove the last element in the array
myArray.pop();
console.log(myArray); // [1, 2, 3, 4]

// Remove the last element in the array
myArray.pop();
console.log(myArray); // [1, 2, 3]

// Remove the last element in the array and print out its value
console.log(myArray.pop()); // 3
console.log(myArray); // [1, 2]
```

# 在数组开头添加和移除元素:

## 。shift()从数组的开头移除一个项目:

`shift()`方法移除数组中的第一个元素，并返回返回元素的值。

```
let myArray = ["A", "B", "C"];
console.log(myArray); // ["A", "B", "C"]

// Remove the 1st element in the array
myArray.shift();
console.log(myArray); // ["B", "C"]

// Remove the 1st element in the array and log its value
console.log(myArray.shift()); // B
console.log(myArray); // ["C"]
```

## 。unshift()将一个项目添加到数组的开头:

`unshift()`方法将一个(或多个)元素添加到数组的开头，并返回数组的新长度。

```
let myArray = ["A", "B", "C"];
console.log(myArray); // ["A", "B", "C"]

// Add an element to the beginning of the array
myArray.unshift("Z");
console.log(myArray); // ["Z", "A", "B", "C"]

// Add multiple elements to the beginning of the array
myArray.unshift(1, 2, 3);
console.log(myArray); // [1, 2, 3, "Z", "A", "B", "C"]

// Review the new length of the array after use of the unshift method
console.log(myArray.unshift(99)); // 8
console.log(myArray); // [99, 1, 2, 3, "Z", "A", "B", "C"];
```

# 使用 splice()添加、移除和替换数组中的元素:

`splice()`方法可以用来添加、移除和替换数组中从给定索引开始的元素。有关`splice()`方法的更多信息，查看关于 splice() 的 [MDN 文档。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

为了帮助解释如何使用`splice()`，请查看下面摘自 MDN 网页的参数的语法和解释:

```
splice(start)
splice(start, deleteCount)
splice(start, deleteCount, item1)
splice(start, deleteCount, item1, item2, itemN)
```

*   start:这是数组中要开始添加、移除或替换元素的元素的索引。基本上就是你的出发点。
*   deleteCount:这是要从起始索引右侧删除的元素的数量。如果不想删除数组中的任何元素，只添加元素，将 deleteCount 的值设置为`0`。
*   item1，item2，… itemN:这些是将使用`splice()`方法插入或替换到数组中的元素。

`splice()`方法返回一个数组，包含从`splice()`操作的数组中移除的所有元素。如果没有删除任何元素，将返回一个空数组。

**注意:**理解`splice()`修改现有数组是很重要的，因为数组是`reference`类型的，如果你不想修改原始数组的内容，你需要小心。

## 使用 splice()在数组内添加元素:

要使用`splice()`方法在数组中添加元素，需要输入起始索引，将第二个参数(deleteCount)设置为 0，然后使用拼接方法输入尽可能多的元素放入数组中。

**注意:**如果您想要在数组中的两个现有元素(比如“a”和“b”)之间添加元素，请确保 splice()方法中的起始索引表示插入右侧元素的当前索引(本例中为“b”)。这是因为`splice()`的工作方式是在起始索引处插入。因此，在执行元素插入时，当前位于起始索引的内容会被推到右边。

```
// Initialize an array
let myArray = ["a", "b", "c", "d"];
console.log(myArray); // ["a", "b", "c", "d"]

// add a number in between "c" and "d".
myArray.splice(3, 0, 1);
console.log(myArray); // ["a", "b", "c", 1, "d"]

// add 2 additional numbers in between 1 and "d"
myArray.splice(4, 0, 2, 3);
console.log(myArray); // ["a", "b", "c", 1, 2, 3, "d"]
```

## 使用 splice()移除数组内的元素:

要使用`splice()`删除数组中的元素，请根据要删除的元素数量将 deleteCount 参数(第二个参数)设置为 1 或更多。您仍然可以将开始索引设置为您希望开始删除的位置。如果你只想用`splice()`删除数组中的元素，不要输入第三个参数。

```
// Initialize an array
let myArray = ["a", "b", "c", "d"];
console.log(myArray); // ["a", "b", "c", "d"]

// Remove "b" from the array
myArray.splice(1, 1);
console.log(myArray); // ["a", "c", "d"]

// Remove "c" and "d" from the array
myArray.splice(1, 2);
console.log(myArray); // ["a"]
```

## 使用 splice()替换数组中的元素:

使用`splice()`方法替换数组中的元素本质上是在数组中添加和移除元素的组合。将开始索引的值设置为您希望开始替换的位置，将第二个参数(deleteCount)设置为您希望从当前数组中移除多少个元素。然后作为第三个+参数，添加一个或多个希望插入数组的元素。

```
// Initialize an array
let myArray = ["a", "b", "c", "d"];
console.log(myArray); // ["a", "b", "c", "d"]

// Replace "b" in the array with 1
myArray.splice(1, 1, 1);
console.log(myArray); // ["a", 1, "c", "d"]

// Replace "d" in the array with 2 and 3
myArray.splice(3, 1, 2, 3);
console.log(myArray); // ["a", 1, "c", 2, 3]
```

# 如何使用获取数组中元素的索引？indexOf():

有些时候，你会想要搜索一个数组，并确定一个特定元素的索引。你可以使用`indexOf()`方法来完成。注意，如果数组中有多个相同值的实例，那么`indexOf()`方法将只返回该值第一次出现的索引。如果`indexOf()`在数组中没有找到指定的值，将返回`-1`。

```
// Initialize an array
let myArray = ["One", "Two", "Three", "Four"];

// Return the index of "Two" in the array
console.log(myArray.indexOf("Two")); // 1

// Return the index of "Four" in the array
console.log(myArray.indexOf("Four")); // 3

// Return the index of "Five" in the array
// Note that "Five" is not in the array
console.log(myArray.indexOf("Five")); // -1
```

`indexOf()`方法可选地接受第二个参数，该参数指定搜索的起始索引。以下取自 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) 的示例显示了如何使用这个可选参数来创建一个数组，该数组包含给定数组中变量“a”的所有实例的索引。

```
// Initialize an empty array indicies which will
// hold all of the indicies of the values of "a"
// found in the "array" variable
var indices = [];

// Create an array with multiple instances of "a"
var array = ["a", "b", "a", "c", "a", "d"];

// Initialize the element in the array variable which
// will be searched for using the indexOf() method
var element = "a";

// Initialize the idx variable which holds the index
// of the 1st occurance of "element" in "array"
var idx = array.indexOf(element);

// Run this loop while idx is not -1
while (idx != -1) {
  // push the current value stored in idx to the indicies
  // array
  indices.push(idx);

  // Update the value of idx with a new value
  // based on the indexOf() method but starting from the
  // next index up of the previous "element" in "array"
  idx = array.indexOf(element, idx + 1);
}
console.log(indices);
// [0, 2, 4]
```

# 如何使用 Sort()方法对数组进行排序:

有一个内置的`sort()`方法可以用来对数组中的元素进行排序。

默认情况下，`sort()`方法遍历数组中的每个元素，将数组中的元素转换为字符串，然后按升序排序。如果你正在排序字符串，这可能是好的，但是如果你在数组中有数字，例如，它们将被转换成字符串并排序。这样做的问题是，转换为字符串的数字是按 Unicode 值排序的，而不是按实际的数值排序的。例如，“100”将排在“8”之前。

为了使用`sort()`方法对数字数组进行正确排序，您需要为它提供一个比较函数或条件。比较函数接受两个参数，分别代表迭代中的当前元素和下一个元素。基本上，比较函数接受两个相邻的值(通常称为“a”和“b”)或元素对。如果比较函数返回一个大于 0 的值，那么第二个元素(“b”)将排在第一个元素(“a”)之前。如果比较函数返回值< =0，则保留第一个和第二个元素的当前顺序。

```
// Define a comparison function that will be used
// to sort the array in ascending order
function compareAscending(a, b) {
  // if a > b, return 1
  // The sort() method will flip the order of a and b
  if (a > b) {
    return 1;
    // Do not flip the order of a and b otherwise
  } else {
    return -1;
  }
}

// Define a comparison function that will be used
// to sort the array in descending order
function compareDescending(a, b) {
  // if a < b, return 1
  // The sort() method will flip the order of a and b
  if (a < b) {
    return 1;
    // Do not flip the order of a and b otherwise
  } else {
    return -1;
  }
}

// Define an array with unsorted numbers
let unsorted = [5, 1, 7, 3, 10, 9, 2, 4, 6, 8];

// Test the sort() method without the use of comparison function
console.log("Using sort() method:");
console.log(unsorted.sort());

// Test the sort() method using the compareAscending function
console.log("Using sort() method with compareAscending:");
console.log(unsorted.sort((a, b) => compareAscending(a, b)));

// Test the sort() method using the compareDescending function
console.log("Using sort() method with compareDescending:");
console.log(unsorted.sort((a, b) => compareDescending(a, b)));
```

## sort()方法是对带有数字的数组进行排序的简写

对于`sort()`方法，有一种使用箭头函数的简便方法来帮助对带有数字的数组进行排序。不要像我们在上面的例子中那样用 if 语句编写比较函数，而是创建一个接受 2 个输入(a 和 b)并返回`a - b`或`b-a`之差的比较函数。返回`a-b`将对数组进行升序排序，而`b-a`将进行降序排序。

```
// Define an array with unsorted numbers
let unsorted = [5, 1, 7, 3, 10, 9, 2, 4, 6, 8];

// Sort the unsorted array in ascending order
let sortedAscending = unsorted.sort((a, b) => a - b);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(sortedAscending);

// Sort the unsorted array in descending order
let sortedDescending = unsorted.sort((a, b) => b - a);
// [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
console.log(sortedDescending);
```

# 反转数组:

使用`reverse()`方法反转数组中元素的顺序。

```
let myArray = [1, 2, 3, 4, 5];
console.log(myArray); // [1, 2, 3, 4, 5]

// Reverse myArray
myArray.reverse();
console.log(myArray); // [ 5, 4, 3, 2, 1 ]

let myArray2 = ["Hi", "my", "name", "is", "joe"];
console.log(myArray2); // ["Hi", "my", "name", "is", "joe"]

// Reverse myArray2
myArray2.reverse();
console.log(myArray2); // [ 'joe', 'is', 'name', 'my', 'Hi' ]
```

# 遍历数组:

有几种方法可以循环遍历数组中的元素。这是用于访问数组中的元素并对其执行操作的常见做法。

## 对于循环:

在 for 循环中使用迭代器来访问数组中的所有(或部分)元素。为了让这个调用数组上的`length`方法来获得数组的长度，并在 for 循环中使用它作为结束条件。

通常，我们将使用 for 循环从头到尾访问数组中的每个元素。然而，我们也可以利用 for 循环的灵活性，只迭代数组中的特定值(例如每隔一个值迭代一次，或者从循环的结尾迭代到循环的开头)

```
// Initialize an array
let myArray = ["One", "Two", "Three", "Four"];

// Iterate over every value in the array
// from start to finish and print it to the console
for (let i = 0; i < myArray.length; i++) {
  console.log(myArray[i]);
}
/*
One
Two
Three
Four
*/

// Iterate over every odd value in the array
// and print it to the console
for (let i = 0; i < myArray.length; i += 2) {
  console.log(myArray[i]);
}
/*
One
Three
*/

// Iterate over every even value in the array
// and print it to the console
for (let i = 1; i < myArray.length; i += 2) {
  console.log(myArray[i]);
}
/*
Two
Four
*/

// Iterate over every value in the array
// starting with the last value and ending in the first
// value, print the result to the console
for (let i = myArray.length - 1; i >= 0; i--) {
  console.log(myArray[i]);
}
/*
Four
Three
Two
One
*/
```

## for…of 循环:

`for...of`循环的工作方式非常类似于标准的`for`循环实现。不是使用迭代器，然后需要使用括号来访问数组的元素，`for...of`的每次迭代都提取数组中的连续元素作为该次迭代的值。

使用`for...of`循环的缺点是不能像使用标准的`for`循环或`forEach()`方法那样访问元素的索引(参见下面关于`forEach()`方法的更多信息)。

```
// Initialize an array
let myArray = ["One", "Two", "Three", "Four"];

// In the for...of condition, during each iteration,
// number, which is the iterator, takes on the value
// of the next element in the specified array
// (myArray in this case)
// So number will take on the value of "One" in the 1st iteration
// Then in the second iteration, it will become "Two",
// followed by "Three", and "Four"
for (let number of myArray) {
  console.log(number);
}
/*
One
Two
Three
Four
*/
```

**注意:**虽然我们不会在这里讨论这个问题，但是`for...of`循环可以用来迭代任何被认为是可迭代的 JS 类型。JS 中可迭代类型的其他例子(除了数组)包括 strings、Map 和 Set。

## forEach()方法:

`forEach()`为数组中的每个元素调用一个函数，从数组中的第 0 个元素开始，一直到最后一个元素。`forEach()`函数可选地允许我们访问被迭代的当前元素的索引。

`forEach()`返回`undefined`。

在数组上调用`forEach()`方法所需要做的就是输入当前迭代元素的名称(例如。“元素”)并使用箭头函数符号来指定您想要为每次迭代运行的函数或代码块。

```
// Initialize an array
let myArray = ["a", "b", "c", "d"];
console.log(myArray); // 4

// Use the forEach() method to log each
// element in the array to the console
myArray.forEach((element) => console.log(element));

// Initialize a new array
let myArray2 = [1, 2, 3, 4, 5];
console.log(myArray2);

// Use the forEach() method to add 1 to each
// element in the array and log it to the console
// Notice that the original values of myArray2 are not changed by the forEach() loop
myArray2.forEach((element) => {
  element += 1;
  console.log(element);
});
console.log(myArray2);

// Use the forEach() method to add 1 to each
// element in the array and log it to the console
// This time, store element in a new array
// We can do this because the forEach() method allows us to optionally include the index of the current element

let newArray = [];

myArray2.forEach((element, i) => {
  element += 1;
  newArray[i] = element;
  console.log(element);
});
console.log(newArray);
```

**注意:**forEach()方法调用的函数被称为回调函数，因为它是从另一个函数内部调用的函数。如果你想了解更多关于回调函数的信息，我推荐 Ania Kubow 制作的这个视频，因为它确实帮助我理解了回调函数是如何工作的，以及它们是如何被调用的。

# 使用现有数组的已修改元素创建新数组。地图():

`map()`方法与`forEach()`方法非常相似。这两种方法的唯一区别是`map()`返回一个新数组，而`forEach()`返回`undefined`。默认情况下，这两个方法都不会修改调用它们的数组。

`map()`方法接受一个回调函数，该函数对数组中的每个元素执行操作。然后，`map()`获取这些修改后的值，并将它们存储在一个新的数组中，然后由`map`方法返回。

```
// Initialize a new array
let myArray = [1, 2, 3, 4, 5];
console.log(myArray); // [ 1, 2, 3, 4, 5 ]

// create a new array using the map method which holds the
//squared values of myArray
let square = myArray.map((element) => element ** 2);
console.log(square); // [ 1, 4, 9, 16, 25 ]

// create a new array using the map method which holds the
// cubed values of myArray
let cube = myArray.map((element) => {
  return element ** 3;
});
console.log(cube); // [ 1, 8, 27, 64, 125 ]
```

# 如何使用移除数组中不需要的元素？过滤器():

`filter()`方法使用回调函数或代码块对数组中的每个元素执行条件测试。然后，通过条件的任何元素都由`filter()`方法放入一个新的数组中。`filter()`方法返回带有“过滤”元素的新数组。如果没有一个元素通过条件测试，将返回一个空数组。

下面的代码向您展示了实现`filter()`方法的多种不同方式:

```
let myArray = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Use the filter method to create an array of even numbers
// from myArray
let even = myArray.filter((element) => element % 2 === 0);
console.log(even);

// Define the isOdd function
function isOdd(number) {
  if (number % 2 !== 0) {
    return number;
  }
}

// Use the filter method to create an array of odd elements
// from myArray by passing in the isOdd() callback function
let odd = myArray.filter((element) => isOdd(element));
console.log(odd);

// Use the filter method to create an array of multiples
// of 3 from myArray by passing in a code block
let multipleOf3 = myArray.filter((element) => {
  if (element % 3 === 0) {
    return element;
  }
});
console.log(multipleOf3);

// Use the filter method to create an array of multiples
// of 5 from myArray
let multipleOf5 = myArray.filter((element) => {
  return element % 5 === 0;
});
console.log(multipleOf5);
```

# 如何使用？查找():

`find()`方法接受一个回调函数，该函数对每个元素执行测试，并返回数组中通过测试的第一个元素的值。

```
let myArray = [1, 2, 3, 4, 5];
console.log(myArray); // [1, 2, 3, 4, 5]

// Find the first element in the array which is greater than 3
let itemFound = myArray.find((element) => element > 3);
console.log(itemFound); // 4

// Set itemFound2 to the first element which is
// greater than 1 and less than 4 (should be 2)
let itemFound2 = myArray.find((element) => {
  if (element > 1 && element < 4) {
    return element;
  }
});
console.log(itemFound2); // 2
```

# 使用复制数组。slice():

使用`slice()`方法复制一个数组或数组的一部分。

*   如果不带任何参数调用，`slice()`方法将创建整个数组的副本。
*   如果只想从指定的索引开始复制数组的一部分，输入起始索引作为`slice()`的第一个参数。
*   您也可以通过输入第二个参数，即停止索引，来控制想要停止`slice()`复制的位置。`slice()`将复制到**的所有内容，但不包括**停止索引。

```
let myArray = [1, 2, 3, 4, 5];
console.log(myArray); // [1, 2, 3, 4, 5]

// Create a copy of myArray
let myArrayCopy = myArray.slice();
console.log(myArrayCopy); // [1, 2, 3, 4, 5]

// Create a copy of the 2nd half of myArray
// starting from the 2nd index (# 3)
let myArray2ndHalf = myArray.slice(2);
console.log(myArray2ndHalf); // [3, 4, 5]

// Create a slice of myArray, only copying
// elements at index 2 through 4
let myArraySlice = myArray.slice(1, 4);
console.log(myArraySlice); // [2, 3, 4]
```

## 在数组内复制对象时 slice()的工作原理:

当使用`slice()`复制数组中的对象时，该对象的*引用*被复制。这意味着，如果您在原始数组或复制数组中更改该对象的值，这种更改将在两个数组中都可以看到，因为由于引用类型的工作方式，这两个数组都指向内存中的同一块数据。

```
let myArray = [["a"], { letter: "b" }, "c", "d", "e"];
let myArrayCopy = myArray.slice();

// [ [ 'a' ], { letter: 'b' }, 'c', 'd', 'e' ]
console.log("myArray:", myArray);

// [ [ 'a' ], { letter: 'b' }, 'c', 'd', 'e' ]
console.log("myArrayCopy:", myArrayCopy);

// Change values of the nested object and array in myArray
myArray[1].letter = "hello";
myArray[0][0] = 200;

// [ [ 200 ], { letter: 'hello' }, 'c', 'd', 'e' ]
console.log("myArray:", myArray);

// [ [ 200 ], { letter: 'hello' }, 'c', 'd', 'e' ]
console.log("myArrayCopy:", myArrayCopy);
```

在上面的例子中，使用`splice()`制作了`myArray`的副本。`myArray`包含索引 0 处的嵌套数组和索引 1 处的嵌套对象。`splice()`创建任何非原始变量类型的浅拷贝(例如对象和数组)。浅层复制意味着复制对内存中值的引用，而不是实际值。所以`myArrayCopy`和`myArray`中的嵌套数组和嵌套对象实际上都指向内存中的同一个值。这意味着当一个值被访问和更改时，该更改会反映在两个副本中。

# 使用 Array.from()转换类似列表的可迭代对象:

`Array.from()`方法将转换一个类似列表的 iterable 对象(例如 NodeList)并将其转换为一个数组。这允许您在转换后的数组上使用所有的数组方法，这有时会很有帮助。

# 其他数组方法

还有很多数组方法我没有在本文中讨论。如果您想了解更多，我建议查看 Mozilla 开发者网络(参见参考资料一节中的链接)。他们对 JavaScript 的所有主题都有很好的详细解释。

# 资源:

*   [MDN —数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
*   [MDN —数组](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Arrays)
*   [MDN —可变](https://developer.mozilla.org/en-US/docs/Glossary/Mutable)
*   [MDN —文字](https://developer.mozilla.org/en-US/docs/Glossary/Literal)
*   [MDN —数组文字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#array_literals)
*   [MDN—array . prototype . length](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length)
*   [MDN—array . prototype . push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
*   [MDN—array . prototype . pop()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
*   [MDN—array . prototype . shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
*   [MDN—array . prototype . un shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)
*   [MDN——用于](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)的……
*   [MDN—array . prototype . foreach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
*   [MDN—array . prototype . index of()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
*   [MDN—array . prototype . sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
*   [MDN—array . prototype . map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
*   [MDN—array . prototype . find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
*   [MDN—array . prototype . reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
*   [Javascript。信息—数组](https://javascript.info/array)
*   [在 JavaScript 中使用数组](https://www.digitalocean.com/community/tutorial_series/working-with-arrays-in-javascript)