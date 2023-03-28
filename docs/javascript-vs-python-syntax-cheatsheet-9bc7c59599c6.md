# Javascript vs Python 语法备忘单

> 原文：<https://medium.com/geekculture/javascript-vs-python-syntax-cheatsheet-9bc7c59599c6?source=collection_archive---------4----------------------->

![](img/4757b1c177979107d5f2965f0f6b256d.png)

在学习了 Ruby 和 Javascript 之后，我一直在寻找一种快速学习 Python 的方法。我需要一个包含两种语言的一些语法差异和高级概念差异的快速备忘单。

通常，当我在网上碰到 Python 代码时，我能够理解其中的逻辑——就像我确信大多数程序员在学习了 Javascript 之后会做的那样。用 python 写代码？那是另外一个故事。

这个备忘单作为两种语言之间的高级比较，帮助您在学习 Javascript 之后开始学习 Python，或者只是想在在线文档上提高您对 Python 代码的理解。将此页面加入书签以方便使用！快乐编程。

**注:**这是用编写时最新版本的 Python 写的——Python 3。

# 空白/区块

## java 描述语言

空白在 Javascript 中没有任何意义。代码块用大括号{ }声明。缩进是为了可读性。

```
if (x == 2) {
  console.log("x must be 2")
} else {
  if (x == 3) {
    console.log("x must be 3")
  } else {
    console.log("x must be something else")
  }
}
```

## 计算机编程语言

在 Python 中，空格的数量告诉计算机什么是函数的一部分，什么不是函数的一部分。您可以使用空格或制表符。如果没有正确的缩进，你会得到一个错误。

```
if x == 2:
  print("x must be 2")
else:
  if x == 3:
    print("x must be 3")
  else:
    print("x must be something else")
```

# **正在评论**

## **Javascript**

单线:

```
// Text after two forward slashes is a comment
```

多行:

```
/* Anything between here is a
comment- this 
can extend several lines*/
```

## **Python**

单线:

```
# Anything after a # is a comment
```

多行:

Python 没有多行注释，但是可以使用它的多行字符串选项。

```
"""
At this point we wish
to print out some numbers
and see where that gets us
"""
```

# 打印到终端

## java 描述语言

```
console.log("print this!")
```

## **Python**

```
print("print this!")
```

# 变量

## java 描述语言

Javascript 有几种方法可以使用“let”或“const”来声明变量，这取决于变量将来是否需要更新(“var”也可以使用，但这是一种过时的声明变量的方式)。

```
let x = "Hello there"
const y = "Good bye"
```

## 计算机编程语言

```
x = "Hello there"
y = "Good bye"
```

# 算术运算符

Javascript 和 Python 之间的算术运算符是相同的——除了一点。Python 缺少前递减/后递减以及前递增/后递增运算符。

## Javascript 额外运算符:

```
-- Pre-decrement, post-decrement
++ Pre-increment, post-increment
```

## Python 额外运算符:

有一点需要注意——由于 Python 将所有数字都视为浮点数(十进制数)，所以可以使用双除法符号//来获得一个整数。这对于查找索引很有用。

```
string1 = "abcde" middle = string1[len(string1) // 2] 
print(middle) # c
```

# 文字布尔值

## java 描述语言

JS 文字布尔使用小写版本。

```
x = true
y = false
```

## 计算机编程语言

PY 文字布尔使用大写版本。

```
x = True
y = False
```

# 布尔运算符

Javascript 和 Python 之间的布尔运算符是相同的——除了两个额外的 JS 运算符。Python 缺少严格的等式/不等式运算符。

## Javascript 额外运算符:

```
=== Strict equality
!== Strict inequality
```

严格平等/不平等的例子:

```
0 == "0"  // true
0 === "0" // false

0 == []   // true
0 === []  // false

0 == 0    // true
0 === 0   // true
```

对于 Python，由于我们没有严格的等式/不等式操作符，这意味着我们需要在进行比较之前将数据转换为相同的类型。

# 逻辑运算符

## java 描述语言

```
!     Logical inverse, not
&&    Logical AND
||    Logical OR
```

## 计算机编程语言

Python 的逻辑操作符易于阅读

```
not    Logical inverse, not
and    Logical AND
or     Logical OR
```

# If 条件句

最显著的区别:Python 使用“elif”而不是“else if”。

## java 描述语言

```
if (x == 10) {
  console.log("x is 10")
} else if (x == 20) {
  console.log("x is 20")
} else {
  console.log("x is something else")
}
```

## 计算机编程语言

```
if x == 10:
  print("x is 10")
elif x == 20:
  print("x is 20")
else:
  print("x is something else")
```

# 数组/列表

*   在 JS 中，它们被称为数组。在 Python 中，它们被称为列表。
*   Python: 2D 列表:heights = [ ["Noelle "，61]，["艾娃"，70]，["Sam "，67]，["Mia "，64] ]

## java 描述语言

创建:

```
let a1 = new Array()   // Empty array
let a2 = new Array(10) // Array of 10 elements
let a3 = []            // Empty array
let a4 = [10, 20, 30]  // Array of 3 elements
let a5 = [1, 2, "b"]   // No problem
```

访问:

```
console.log(a4[1])  // prints 20

a4[0] = 5   // change from 10 to 5
a4[20] = 99 // OK, makes a new element at index 20
```

元素的长度/数量:

```
a4.length; // 3
```

添加/删除项目

```
fruits.push("Kiwi") // adds Kiwi to list 
fruits.pop("Kiwi")  // removes Kiwi from list
```

## 计算机编程语言

创建:

```
a1 = list()          # Empty list
a2 = list((88, 99))  # List of two elements
a3 = []              # Empty list
a4 = [10, 20, 30]    # List of 3 elements
a5 = [1, 2, "b"]     # No problem
```

访问:

```
print(a4[1])  # prints 20

a4[0] = 5;    # change from 10 to 5
a4[20] = 99;  # ERROR: assignment out of range
```

元素的长度/数量:

```
len(a4)   # 3
```

添加/删除项目

```
fruits.append("Kiwi")  # adds Kiwi to list 
fruits.remove("Kiwi")  # removes Kiwi from list
```

额外收获:如何从 2D 名单中除名

```
customer_data = [["Ainsley", "Small", True],["Ben", "Large", False]]
customer_data[1].remove("Ben")# removes "Ben" from second array
# [["Ainsley", "Small", True],["Large", False]] 
```

## 元组

Python 支持名为*元组*的只读类型的列表。它是不可改变的——你不能改变或修改它。

```
x = (1, 2, 3)
print(x[1])  # prints 2y = (10,)    
# A tuple of one element, comma required
```

# 列出方法

使用列表的其他方式

## **插入**:

从列表的特定索引插入元素的列表方法

**Javascript** 。拼接()

```
fruits.splice(1, 0, "orange")// will insert the value "orange" as the second element of the fruit list (deleting 0 items first)
```

**Python** 。插入()

```
fruits.insert(1, "orange")# will insert the value "orange" as the second element of the fruit list
```

## **流行:**

从特定索引或列表末尾移除元素的 list 方法。

**Javascript** 。流行()

在 Javascript 中，此方法只能用于移除列表中的最后一个元素。

```
fruits.pop()// will remove and return the last element from the list 
```

**巨蟒**。流行()

在 Phython 中，这个方法可以用来从特定的索引中删除一个元素，或者列表中的最后一个元素。

```
fruits.pop()
# will remove and return the last element from the listfruits.pop(2)
# will remove and return the element with index 2 from the list
```

## 切片:

仅提取列表一部分的列表方法。

**Javascript** 。切片()

```
const fruits= ["Banana", "Orange", "Lemon", "Apple", "Mango"]const slicedFruits = letters.slice(1, 3)
console.log(slicedFruits)// ["Orange", "Lemon"]
```

**Python** 【开始:结束】

在 Python 中，我们使用以下语法执行切片:[start: end]。`start`是我们希望包含在选择中的第一个元素的索引。`end`是不是*的索引比我们要包含的最后一个索引*多一个。

```
fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"]sliced_fruits = fruits[1:3]
print(sliced_fruits)# ["Orange", "Lemon"]
```

Python 中的切片非常灵活。如果我们想选择列表中第*`*n*`*元素*或者列表中最后*`*n*`*元素*，我们可以使用下面的代码:**

```
**fruits[:n]
fruits[-n:]**
```

**对于我们的`fruits`列表，假设我们想要去掉前三个元素。**

```
**fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"]print(fruits[:3])
# ["Banana", "Orange", "Lemon"]print(fruits[-2:])
# ["Apple", "Mango"]**
```

**我们也可以处理列表中除最后 n 个元素之外的所有元素:**

```
**fruits[-n:]print(fruits[:-1])
# ["Banana", "Orange", "Lemon", "Apple"]**
```

## **排序:**

**对列表进行排序的方法和内置函数。Javascript 和 Python 都有按字母或数字排序的`.sort()`方法。**

****Javascript** 。排序()**

****注:**如果数字按字符串排序，“25”大于“100”，因为“2”大于“1”。**

```
**const names = ["Xander", "Buffy", "Angel", "Willow", "Giles"]names.sort()
console.log(names)// ['Angel', 'Buffy', 'Giles', 'Willow', 'Xander']**
```

****巨蟒**。sort() /。已排序()**

**。sort() Sort 直接修改列表。**

```
**names = ["Xander", "Buffy", "Angel", "Willow", "Giles"]names.sort()
print(names)# ['Angel', 'Buffy', 'Giles', 'Willow', 'Xander']**
```

**`.sort()`还为我们提供了轻松反转的选项。我们可以按降序排序，而不是按升序排序。**

```
**names = ["Xander", "Buffy", "Angel", "Willow", "Giles"]names.sort(reverse=True)
print(names)# ['Xander', 'Willow', 'Giles', 'Buffy', 'Angel']**
```

**。sorted()与`.sort()` 不同，因为它出现在列表之前，而不是之后。并且，它生成一个新的列表，而不是修改一个已经存在的列表。**

```
**names = ["Xander", "Buffy", "Angel", "Willow", "Giles"]sorted_names = sorted(names)
print(sorted_names)# ['Angel', 'Buffy', 'Giles', 'Willow', 'Xander']**
```

## **Python 的优势:**

****。范围()****

**创建整数序列的内置 python 函数。函数`range()`接受单个输入，并生成从`0`开始并在输入之前的数字**结束的数字。****

```
**my_range = range(10)
print(my_range)
# range(0, 10)**
```

**这将创建一个*范围对象。*如果我们想要使用这个对象作为列表，我们需要使用一个名为`list()`的内置函数来转换它。**

```
**print(list(my_range))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]**
```

**我们也可以用两个输入调用`range()`，并创建一个从不同数字开始的列表。**

```
**my_list = range(2, 9)
print(list(my_list))
# [2, 3, 4, 5, 6, 7, 8]**
```

**我们也可以用三个输入调用`range()`，并创建一个“跳过”数字的列表。例如，我们将从`1`开始，在每个数字之间以`10`的增量跳过，直到到达`100`:**

```
**my_range3 = range(1, 100, 10)
print(list(my_range3))
# [1, 11, 21, 31, 41, 51, 61, 71, 81, 91]**
```

****。伯爵()****

**与 Javascript 不同，Python 有一种内置的方法来计算列表中某项的出现次数，而不必为它编写函数。**

```
**letters = ["m", "i", "s", "s", "i", "s", "s", "i", "p", "p", "i"]num_i = letters.count("i")
print(num_i)# 4**
```

**这个方法甚至可以计算 2D 列表中元素的出现次数。**

```
**number_collection = [[100, 200], [100, 200], [475, 29], [34, 34]]num_pairs = number_collection.count([100, 200])
print(num_pairs)# 2**
```

# **对于循环**

**迭代。**

****Javascript****

```
**for (let i = 0; i < 10; i++) {
  console.log(i)
}**
```

****Python****

**我们可以用`range()`函数来计数:**

```
**for i in range(10):
  print(i) # Prints 0-9**
```

**或者，我们可以迭代类型:**

```
**a = [10, 20, 30]

# Print 10 20 30
for i in a:
  print(i)

# A dictionary
b = {'x':5, 'y':15, 'z':0}

# Print x y z (the keys of the dict)
for i in b:
  print(i)**
```

# **While 循环**

**当条件为真时。**

****Javascript****

```
**let x = 10
while (x >= 0) {
  console.log(x)
  x--
}// prints 10 down to 0**
```

****Python****

```
**x = 10
while x >= 0:
  print(x)
  x -= 1# prints from 10 down to 0**
```

# **功能**

**定义函数。**

****Javascript****

```
**function foobar(x, y, z) {
  console.log(x, y, z)
  return 12
}**
```

**箭头功能**

```
**let hello = () => {
  console.log("hello")
  console.log("world")
}// prints hello, then world**
```

****Python****

```
**def foobar(x, y, z):
  print(x, y, z)
  return 12**
```

# **用线串**

**Python 和 Javascript 中的字符串都使用单引号和双引号。两者之间的字符串方法确实不同。这里有几个例子:**

****Javascript** 。toUpperCase() / toLowerCase()**

**`.toUpperCase()`返回全大写字符的字符串。**

```
**let str = "Hello World!"
str.toUpperCase()// HELLO WORLD!**
```

**`.toLowerCase()`返回全部为小写字符的字符串。**

```
**let str = "Hello World!"
str.toLowerCase()// hello world!**
```

****Python** 。upper() /。下部()**

**`.upper()`返回全大写字符的字符串。**

```
**str = "Hello World!"
str.upper()# HELLO WORLD!**
```

**`.lower()`返回全部为小写字符的字符串。**

```
**str = "Hello World!"
str.lower()# hello world!**
```

**你可以在这里看到可用 Python 字符串方法的列表[。](https://www.python-ds.com/python-3-string-methods)**

# **对象/词典**

**在 Javascript 中，对象保存的数据可以使用一个名为*属性*的键找到。在 Python 中，这些键/值对被称为字典。**

****Javascript****

```
**let food1 = {}                 // empty object
let food2 = {pizza: "tomato"}  // property quotes optional// common multiline format
let prices= {  
  "pizza": 20,
  "pasta": 1.2,
  "drink": "free"
}**
```

**访问:**

```
**console.log(prices.pizza)      // prints 20
console.log(prices["drink"])   // prints free**
```

****Python****

```
**food1 = {}                   # empty dict
food2 = {"pizza": "tomato"}  # key quotes are required# multiline format
prices = {  
  "pizza": 20,
  "pasta": 1.2,
  "drink": "free"
}**
```

**访问:**

```
**print(prices["pizza"])  # Prints 20**
```

# **班级**

**类是数据类型的模板。Javascript 和 Python 都使用带有大写约定的`class`关键字。**

****Javascript****

**现代 JS 引入了`class`关键字和大多数其他 OOP 语言更熟悉的语法。注意，继承模型仍然是原型继承。**

```
**class Creature {
  constructor(type) {
    this.type = type
  }
}**
```

**访问:**

```
**class Goat extends Creature {
  constructor(name) {
    super("mammal")
    this.legs = 4
    this.name = name
  }

goat = new Goat("billy")
goat.type                 // "mammal"
goat.name                 // "billy"
goat.jump()               // "I'm jumping! Yay!"**
```

****巨蟒****

**Python 有一个类构造器方法，叫做“dunder”(例如:`__init__()`)。这代表*双下划线。***

```
**class Shouter:
  def __init__(self):
    print("HELLO?!")class Creature:
  def __init__(self, type): # constructor
    self.type = type**
```

**访问:**

**你可以用`object.variable`语法访问一个对象的所有类变量。**

```
**class Musician:
  title = "Rockstar"

drummer = Musician()
print(drummer.title)# prints "Rockstar"**
```

****注意:**还有另一个 dunder 方法叫做`__repr__()`。这是一个我们可以用来告诉 Python 我们希望类的*字符串表示*是什么的方法。**

```
**class Employee():
  def __init__(self, name):
    self.name = name

  def __repr__(self):
    return self.name

argus = Employee("Argus Filch")
print(argus)# without using __repr__() 
# printed "<__main__.Employee object at 0x104e88390>"# after using __repr__()
# prints "Argus Filch"**
```