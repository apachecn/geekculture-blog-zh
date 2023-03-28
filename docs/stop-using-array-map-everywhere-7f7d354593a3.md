# 停止在🥵到处使用 Array.map()

> 原文：<https://medium.com/geekculture/stop-using-array-map-everywhere-7f7d354593a3?source=collection_archive---------19----------------------->

![](img/cd44f8538ede95f78d6d440fa7f97d10.png)

大多数时候我会看到这样的片段👇

```
const fruits = ["apple", "orange", "cherry"];
let text = "";
document.getElementById("main").innerHTML = text;
fruits.map(i => text += i );
```

在上面的代码片段中，我们将`fruits`文本添加到 DOM 的`main` ID 中。
上面的片段似乎没有问题，但有一个主要问题，我们今天将会看到。

让我们通过`map`的定义来理解这个问题，`map()`方法创建了一个新的数组，其中填充了调用数组中每个元素的函数的结果。

**示例:**

```
let n = [1, 2, 3, 4];
let add = n.map(i => i + 2);
console.log(add); // [3, 4, 5, 6]
```

**注意:**使用`map()`方法意味着返回一个数组的新集合。

如前所述，`map()`方法总是返回一个新数组，所以如果你不需要使用新数组，就不要使用`map()`方法。
当你只需要迭代一个数组的时候，我会一直推荐使用其他的数组方法比如`forEach`或者`for..of`。

**例如:**

```
const fruits = ["apple", "orange", "cherry"];
let text = "";
fruits.forEach(myFunction);document.getElementById("main").innerHTML = text;function myFunction(item, index) {
  text += index + ": " + item + "<br>"; 
}
```

# 我们为什么关心它？🙄

我们知道，`map()`方法总是返回一个数组。如果您只需要更新 DOM，那么将这些元素存储到内存形式没有任何意义。

当然，对于一小部分数字来说，什么都不会发生，但是，如果我们在这里取一个较大的数字，那么它会影响性能方面，因为它会将值存储在冗余的内存中。

# 总结⅀

如果你只需要遍历一个数组，停止使用`map()`方法。
如果你想遍历一个数组，开始使用`forEach`或`for...of`方法。

感谢您阅读❤️这篇文章

🌟[推特](https://twitter.com/suprabhasupi) |📚[电子书](https://gum.co/css-pseudo-class-elements)🌟 [Instagram](https://www.instagram.com/suprabhasupi/)