# JavaScript ES6-数组和对象析构。

> 原文：<https://medium.com/geekculture/javascript-es6-array-and-object-destructuring-end-to-end-b3f23cb9968f?source=collection_archive---------30----------------------->

![](img/7bdf9e55d5b3e3b8abda8d47184d9074.png)

ES6 new Features destructuring

# JavaScript 中的这种析构是什么？

> 从数组中获取元素或从对象中获取属性，并将它们设置为局部变量。可以从数组中解包值，或者从对象中解包属性，这是一种非常强大的方式来**将值分配为 ES6 (ES2015)中的不同变量**。我们主要可以把这些解构分为两部分。

1.  ***破阵。***
2.  ***物体毁坏。***

在旧的 javascript 风格中，我们用一行从数组和对象中提取每个元素。

```
let array = [ 5, 6, 9, 1, 3, 4, 8];
const valOne = array[0];console.log(valOne);
***// output: 5***const planet = {  
name: 'Hosnian Prime',  
faction: 'New Republic',  
weather: 'Temperate'
};const plantName = planet.name;
console.log(plantName); 
***// Output: 'Hosnian Prime'***
```

因此，如果一个对象有 100 个元素，我们必须写 100 行来提取，但是使用 ES6 析构，我们可以在一行中解决这个问题。用一行代码，我们可以从一个对象或一个数组中提取多个元素。

```
const organizations = ['Pyke', 'Black Sun', 'Kanjiklub', 'Crimson'];
const [firstGang, secondGang, ...rest] = organizations;console.log(firstGang); ***// output: "Pyke"***
```

**这个技巧可以让你的 JS 代码更简洁，可读性更强。**

# 数组析构

如果希望从指定数组中获取一个元素作为单独的非重复变量。你可以利用**析构**来用一行代码实现你的目标。

简单地说，常规的方法是访问数组中的一个元素。首先，我们迭代给定的数组，并逐个访问每个元素。但是当使用这个数组析构时，我们不需要使用任何索引或循环。这就是 **ES6** 中 ***破阵*** 的妙处。

不使用变量名。相反，我们可以使用类似于语法的对象析构——通过用索引访问元素。如果你愿意，你也可以设置**默认值**！这样，即使传递的数组没有足够的值，所有的变量都有一个定义的值

```
const arr = ['Pyke', 'Black Sun', 'Kanjiklub', 'Crimson'];const {0: first , 3: fourth} = arr;console.log(first) ***//output: 'Pyke'***
console.log(fourth) ***//output: 'Crimson'***const {0: first, 3: fourth, 9: tenth = 'moon'} = arr;
console.log(tenth) ***//output: 'moon'***
***// here index number 10th is undifine so, default value will assign for this tenth variable.***
```

# 交换变量

通常，我们必须使用临时变量进行交换。

```
let a = 1;
let b = 2;
let temp;

temp = a;
a = b;
b = temp;console.log(a) ***//output: 2***
console.log(b) ***//output: 1***
```

你能相信吗，我们可以用下面的代码这样做？

```
let a = 1;
let b = 2;**[a, b] = [b, a];**console.log(a) ***//output: 2***
console.log(b) ***//output: 1***
```

使析构更加有趣的是交换 n 个变量的能力:

```
**[a, b, c] = [b, c, a]**
```

# 使用 Rest 语法(…)省略元素

rest 语法的意思是选取多个元素，并将它们放入一个新元素中。当析构时，这对于移除某个值很方便。也可以使用下面的表达式克隆数组。

```
const arr = ["Hello", "How" , "are", "you"];
const [ hello , , ...others ] = arr;console.log(others)***//output: ["are", "you"]***
```

# 组合多个数组并将现有数组分配到一个新数组中。

通常，有几种方法可以将多个数组组合在一起。通常我们可以使用 ***。*** 的 concat()方法在 javascript 中合并两个数组。

```
const hege = ["Cecilie", "Lone"];
const stale = ["Emil", "Tobias", "Linus"];
const children = hege.concat(stale);console.log(children); 
***//output: ["Cecilie", "Lone","Emil", "Tobias", "Linus"]***
```

但是使用 ES6 析构，我们可以将多个数组组合成一个新数组，如下例所示。

```
const hege = ["Cecilie", "Lone"];
const stale = ["Emil", "Tobias", "Linus"];
const newl = ["Singh", "Shakya"];const newArray = [...hege , ...stale , ...newl ];
console.log(newArray);***//output: ["Cecilie","Lone","Emil","Tobias","Linus",******"Singh", "Shakya"******]***
```

# 数组析构中的常见用例

当我们要写一个函数时。需要将数组作为参数传递，但您希望只使用给定数组中的第一个和第二个元素。你必须返回加法值和乘法值。那么，下面的用例将是这个场景的一个很好的例子。

在这里，我使用析构交换工具演示了冒泡排序算法，

Bubble Sort Algorithm

# 对象析构

对象的析构允许你将变量绑定到对象的不同属性。首先指定要绑定的属性，然后指定值将绑定到的变量。你会注意到这很像我们对数组所做的。

正如你在上面的例子中看到的，我们给变量命名的方式和给对象的键命名的方式是一样的。尽管如此，可以定义名称**不同于关键字**的变量。

```
const planet = {  
name: 'Hosnian Prime',  
faction: 'New Republic',  
weather: 'Temperate'
};const { 
name: system, 
faction: team, 
weather: conditions } = planet;console.log(system); ***// Output: 'Hosnian Prime'***
console.log(team); ***// Output: 'New Republic'***
console.log(conditions); ***// Output: 'Temperate'***
```

## 我们也可以像下面的例子一样设置**的默认值。**

```
const planet = {  
name: 'Hosnian Prime'
};const { 
name: system = 'default planet', 
faction: team = 'default faction', 
weather: conditions = 'default conditions'
} = planet;console.log(system); ***// Output: 'Hosnian Prime'***
console.log(team); ***// Output: '*default faction*'***
console.log(conditions); ***// Output: '*default conditions*'***
```

# 组合多个对象并将现有对象分配到一个新对象中。

这是对象合并中非常特殊的情况。在对象中【属性】也是**唯一的**和**关联的单个值。**根据 **ES6** 特性当我们给一个现有的属性赋值时。新值将被以前的值覆盖。

这篇文章恰当地解释了**对象**如何在 ES6 中工作。

[](https://dtsangeeth.medium.com/count-duplicates-in-array-using-javascript-map-and-object-es6-new-features-a22bd9fd6a16) [## 使用 JavaScript 映射和对象计算数组中的重复项(ES6 新特性)

### 算法和数据结构(JavaScript)

dtsangeeth.medium.com](https://dtsangeeth.medium.com/count-duplicates-in-array-using-javascript-map-and-object-es6-new-features-a22bd9fd6a16) 

然后，如果有每个属性将覆盖的**唯一键**。下面的代码示例解释了它是如何在 JS 中工作和实现的。

# 对象析构中的常见用例

正如我前面提到的，如果我们调用函数时需要传递对象变量作为参数。然后，下面的例子将指导如何实现它，以及如何在用例中使用析构。在这里，我演示了如何访问嵌套属性，以及如何为指定对象中的每个属性设置默认值。

谢谢大家！