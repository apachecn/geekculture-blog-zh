# JavaScript 数组方法

> 原文：<https://medium.com/geekculture/javascript-array-methods-bb48e62b3d21?source=collection_archive---------13----------------------->

![](img/c9e84d5b8cec9fcf52533b900829658f.png)

JavaScript Array 类是一个全局对象，用于构建高级的类似列表的对象数组。

JavaScript 数组的优势在于数组方法。数组方法是内置的 JavaScript 方法，它使编码更容易，也使您的代码看起来简洁易懂。

# 用 JavaScript 声明一个数组

创建空数组有两种语法。

```
// First Syntax
let arr = new Array();//Second Syntax
let arr = [];
const fruits = ["Apple", "Orange","Plum"];
```

几乎总是使用第二种语法。我们可以在方括号中提供初始元素。

# 数组方法

现在我们已经声明了我们的数组，让我们深入研究数组方法:

1.  **地图()**

此方法创建一个新数组，其结果是为每个数组元素调用一个函数。它为数组中的每个元素依次调用一次提供的函数。

示例:

```
const numbers = [5, 10, 15, 20];
const numbersMap = numbers.map(element => element * 5);
console.log(numbersMap); //[25, 50, 75, 100];
```

2. **concat()**

此方法用于连接两个或多个数组。它不会更改现有数组，但会返回一个新数组，其中包含联接数组的值。

示例:

```
const arr1 = [1,2,3];
const arr2 = [4,5,6];
const arr3 = arr1.concat(arr2);
console.log(arr3); //[1,2,3,4,5,6]
```

3. **copyWithin()**

此方法将数组元素复制到数组中的另一个位置，覆盖现有的值。

该方法永远不会向数组中添加更多的项。

示例:

```
const numbers = [1,2,3,4];
const numbersCopy = numbers.copyWithin(2,0);
console.log(numbersCopy); // [1,2,1,2];
```

4. **every()**

此方法检查数组中通过条件的每个元素，根据情况返回 true 或 false。

如果找到函数返回 false 值的数组元素，every()将返回 false(并且不检查其余的值)。

如果没有出现 false，则 every()返回 true。

示例:

```
const numbers = [1,2,3,4];
const isGreaterThanFive = numbers.every(num => num > 5);
console.log(isGreaterThanFive); // falseconst isLessThanFive = numbers.every(num => num < 5);
console.log(isLessThanFive); // true
```

5. **fill()**

此方法用静态值填充数组中的指定元素。我们可以指定*开始*和*结束*灌装的位置。如果未指定，将填充所有元素。

此方法覆盖原始数组。

示例:

```
const withoutStartEnd = [1,2,3,4];
withoutStartEnd.fill(5);
console.log(withoutStartEnd); //[5,5,5,5]const withoutEnd = [1,2,3,4];
withoutEnd.fill(5,2);
console.log(withoutEnd); //[1,2,5,5]const withStartEnd = [1,2,3,4];
withStartEnd.fill(5,1,2);
console.log(withStartEnd); //[1,5,3,4]
```

6.**过滤器()**

此方法创建一个填充了所有数组元素的数组，该数组传递所提供函数中的一个条件。

示例:

```
const numbers = [1,2,3,4];
const filtered = numbers.filter(element => element === 2);
console.log(filtered); //[2]
```

7.**查找()**

此方法返回数组中通过条件的第一个元素的值。它对数组中的每个元素执行一次函数。

示例:

```
const numbers = [10,20,30,40];
const findNumber = numbers.find(element => element < 30);
console.log(findNumber); // 10
```

8. **findIndex()**

此方法返回传递条件的数组中元素的索引。它对数组中的每个元素执行一次函数。

如果条件失败，它简单地返回 **-1** 。

示例:

```
const numbers *=* [1,2,3,4];
const indexFound *=* numbers.findIndex(element => element *===* 3);
console.log(indexFound); //2const indexNotFound = numbers.findIndex(element => element === 5);
console.log(indexNotFound); // -1
```

9. **indexOf()**

此方法在数组中搜索指定项，并返回其位置。搜索将从指定的位置开始，如果没有指定起始位置，则从开头开始，并在数组的末尾结束搜索。

如果没有找到该项目，它只是返回 **-1** 。

示例:

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
const indexFound *=* fruits.indexOf('Mango');
console.log(indexFound); *// 1*const indexNotFound *=* fruits.indexOf('Pineapple');
console.log(indexNotFound); *// -1*
```

10.**包括()**

此方法确定数组是否包含指定的元素。如果数组包含该元素，则返回*真*，否则返回*假*。

示例:

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
const fruitsInclude *=* fruits.includes('Mango');
console.log(fruitsInclude); // true
```

11. **shift()**

此方法移除数组的第一个元素，并返回新的长度。它改变了原始数组。

示例:

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
fruits.shift();
console.log(fruits); // ['Mango','Orange','Banana']
```

12. **unshift()**

此方法向数组的开头添加新项，并返回新的长度。它改变了原始数组。

示例:

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
fruits.unshift('Pineapple');
console.log(fruits); // ['Pineapple', 'Apple', 'Mango', 'Orange', 'Banana']
```

13.**拼接()**

此方法在数组中添加/移除项，并返回移除的项。

示例:向数组添加项目。

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
fruits.splice(2, 0, "Lemon", "Kiwi");
console.log(fruits); // ['Apple', 'Mango', 'Lemon', 'Kiwi', 'Orange', 'Banana']
```

示例:从数组中移除项目

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
fruits.splice(2, 1);
console.log(fruits); // ['Apple', 'Mango', 'Banana']
```

14. **slice()**

此方法将数组中的选定元素作为新的 array 对象返回。

此方法选择从给定的开始参数开始，到给定的结束参数结束的元素，但不包括给定的结束参数。

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
const newFruits *=* fruits.slice(1, 3);
console.log(newFruits); // ['Mango', 'Orange']
```

15. **sort()**

此方法对数组中的项进行排序。排序顺序可以是*字母*或*数字*，也可以是*升序*或*降序。*

默认情况下，此方法按字母和升序将值排序为字符串。

示例:

```
const fruits *=* ['Apple', 'Mango', 'Orange', 'Banana'];
ruits.sort();
console.log(fruits); // ['Apple', 'Banana', Mango', 'Orange']const numbers *=* [25, 10];
numbers.sort();
console.log(numbers); // [10, 25]
```

# 结论

为了在数组中进行操作，我们应该使用这些数组方法来简化我们的工作，这有助于编写干净的代码。

要阅读更多关于这些方法的内容，以下是一些参考资料。

*   [MDN 网络文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
*   [W3Schools](https://www.w3schools.com/jsref/jsref_obj_array.asp)

感谢您的阅读。