# JavaScript:如何在数组中按值删除元素？

> 原文：<https://medium.com/geekculture/javascript-how-to-delete-an-element-by-value-in-an-array-a39595ffa698?source=collection_archive---------0----------------------->

使用 JavaScript 中的现有方法从数组中删除元素

![](img/8b61b1ba51a60453c968d02931a0caa7.png)

Photo by [Bharat Patil](https://unsplash.com/@bharat_patil_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在 JavaScript 中，要从数组中删除一个项目，我们有以下方法:

1) pop()

从 JavaScript 数组的末尾移除一个元素。

2) shift()

从 JavaScript 数组的开头移除一个元素。

3)拼接()

移除 JavaScript 数组中指定索引处的元素。

4)删除运算符(例如:delete arr[3])

删除 JavaScript 数组中指定索引处的元素。但是， **null** 将被插入该索引，因为它不会改变数组的长度。

## **我们有哪些选项可以通过值移除元素并自动调整数组大小？**

a)使用功能的**拼接**和**索引**

设 arr = ['一'，'二'，'三'，'四']
删除一个值为'三'的元素。

arr.splice(arr.indexOf('Three ')，1)；
console . log(arr)；

b)使用**过滤器**功能

设 arr = ['一'，'二'，'三'，'四']
删除一个值为'三'的元素。

设 filteredArr = arr。 **filter** (function(value，index，arr){返回值！= =‘三’；});

注意，上面的过滤函数返回一个新的数组。

**感谢**阅读！请👏如果你喜欢这篇文章，请跟我来，因为它鼓励我写更多！