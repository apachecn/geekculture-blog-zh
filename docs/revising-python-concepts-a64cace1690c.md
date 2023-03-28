# 一些很酷的 Python 代码片段可以加速你的 Python 知识

> 原文：<https://medium.com/geekculture/revising-python-concepts-a64cace1690c?source=collection_archive---------10----------------------->

重温 python 的基本概念，包括让你的代码更 python 化的技巧和诀窍。

下面所有的代码片段都使用 Python 版本 **3.5+**

![](img/8a310f5d5472be5e079f75e4170f9bca.png)

Photo by [Lukas Blazek](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 将两个列表合并成一个字典

```
keys = ['a', 'b', 'c']
values = [1, 2, 3]
dictionary = dict(zip(keys, values))
print(dictionary)# output: {'a': 1, 'b': 2, 'c': 3}
```

# 将两本词典合二为一

```
x = {"a": 1, "b": 2}
y = {"b": 3, "c": 4}z = {**x, **y}
print(z)# output: {'a': 1, 'b': 3, 'c': 4}
```

# 索引字符串

打印字符串的最后一个字符

```
a = "abcd"x1 = a[-1]      # last item in the array
x2 = a[-2:]     # last two items in the array
x3 = a[:-2]     # everything except the last two items
x4 = a[::-1]    # all items in the array, reversed
x5 = a[1:3]     # index 1,2 (excluding 3)print(x1)       # OUTPUT: d
print(x2)       # OUTPUT: cd
print(x3)       # OUTPUT: ab
print(x4)       # OUTPUT: dcba
print(x5)       # OUTPUT: bc
```

# 追加与扩展 Python 列表

将一个列表附加到另一个列表

```
x = [1, 2, 3]
y = [4, 5]
x.append(y)
print(x)# output: [1, 2, 3, [4, 5]]
```

将一个列表扩展到另一个列表

```
x = [1, 2, 3]
y = [4, 5]
x.extend(y)
print(x)# output: [1, 2, 3, 4, 5]
```

向列表中添加另一个项目

```
prog_language = ['Python', 'Java']
prog_language.append('Dart')
print(prog_language)# output: ['Python', 'Java', 'Dart']
```

合并两个列表

```
num1 = [4, 5, 6]
num2 = [5, 6, 7]result = num1 + num2
print(result)# output: [4, 5, 6, 5, 6, 7]
```

# Python 中的切换大小写

与其他编程语言不同，Python 没有 switch 或 case 语句。为了避开这个事实，我们可以使用 python 的字典映射。

```
def numbers_to_strings(argument):
    # argument: key of a dictionary switcher = {
        0: "zero",
        1: "one",
        2: "two",
    }
    return switcher.get(argument, "Data Not Available") if __name__ == "__main__": result = numbers_to_strings(1)
    print(result)                        # output: one
```

当密钥不可用时

```
def numbers_to_strings(argument):
    switcher = {
        0: "zero",
        1: "one",
        2: "two",
    }
    return switcher.get(argument, "Data Not Available") if __name__ == "__main__": result = numbers_to_strings(4)
    print(result)# output: Data Not Available
```

# 打开文本文件

我们可以打开如下所示的文件:

```
file = open('filename.txt')
```

但是当我们打开一个文件时，我们也应该在处理完它后关闭它。有两种方法可以确保打开文件后将其关闭。

方法 1:使用`try-finally`块

```
file = open('filename.txt')
try:
    # do whatever you want to do with your file.
finally:
    file.close()
```

方法二:使用`with`语句。`with`打开前的关键字负责正确关闭文件。

```
with open('filename.txt') as my_file:
    # do whatever you want to do with your file.
```

# 在函数中使用生成器

```
x = sum(i for i in range(10))
print(x)                         # output: 45
```

# 转置矩阵

```
x = [[31, 17], [40, 51], [13, 12]]
print(list(zip(*x)))
```

上面代码的输出

```
[(31, 40, 13), (17, 51, 12)]
```

# 列表的内置函数

**反转()**

```
language = ["Python", "Java", "Dart"]
language.reverse()
print(language)# output: ['Dart', 'Java', 'Python']
```

**插入()**

```
x = [1, 2, 3, 4, 5, 6, 7, 8]
x.insert(0, 10)               # insert 10, at position 0
print(x)# output: [10, 1, 2, 3, 4, 5, 6, 7, 8]
```

**count()** |统计列表中某一项出现的总次数

```
x = [1, 2, 3, 4, 1, 1]
print(x.count(1))# output: 3
```

**clear()** |从列表中删除所有项目

```
x = [1, 2, 3, 4, 1, 1]
x.clear()
print(x)# output: []
```

**index()** |返回某个元素的索引

```
x = ["a", "b", "c", "d", "e", "f"]
print(x.index("d"))# output: 3
```

**len()** |返回列表的总长度

```
x = ["a", "b", "c", "d", "e", "f"]
print(len(x))# output: 6
```

**pop()** |删除并返回列表的最后一项

```
x = ["a", "b", "c", "d", "e", "f"]print(x.pop())             # output: fprint(x)                   # output: ['a', 'b', 'c', 'd', 'e']
```

**sort()** |对列表中的元素排序

```
x = [2, 1, 3, 5, 4]
x.sort()                # sort in ascending order
print(x)                # output: [1, 2, 3, 4, 5]y = [2, 1, 3, 5, 4]
y.sort(reverse=True)    # sort in descending order
print(y)                # output: [5, 4, 3, 2, 1]
```

从列表中删除某个元素

```
x = [2, 1, 3, 5, 4, 3, 3]
x.remove(3)                      # remove the first occurrence of 3
print(x)                         # output: [2, 1, 5, 4, 3, 3]
```

# 从列表中删除项目的所有出现

方法一；使用 lambda 函数

```
# remove all 2 from the list a = [1, 2, 3, 2, 2, 2, 3, 4]
b = filter(lambda x: x != 2, a)
print(list(b))# output: [1, 3, 3, 4]
```

方法二；使用内置过滤器

```
a = [1, 2, 3, 2, 2, 2, 3, 4]
b = filter((2).__ne__, a)
print(list(b))# output: [1, 3, 3, 4]
```

# 从整数/字符串列表中查找最小值/最大值

如果是 List[int]

```
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]min_a = min(a)
max_a = max(a)print(min_a)       # OUTPUT: 1
print(max_a)       # OUTPUT: 10
```

在 List[str]的情况下；基于*字典序*

```
a = ["flower", "flow", "flights"]min_a = min(a)
max_a = max(a)print(min_a)       # OUTPUT: flights
print(max_a)       # OUTPUT: flower
```

在 List[str]的情况下；基于字符串*长度*

```
a = ["flower", "flow", "flights"]min_a = min(a, key=len)
max_a = max(a, key=len)print(min_a)      # OUTPUT: flow
print(max_a)      # OUTPUT: flights
```

# 移除、删除、弹出之间的区别

**remove()** |删除第一个匹配值，而不是特定的索引

```
x = [2, 1, 4, 3]
x.remove(1)
print(x)                            output: [2, 4, 3]
```

**del** |删除特定索引处的项目

```
x = [2, 1, 4, 3]
del x[0]                           # delete the item in index 0
print(x)                           # output: [1, 4, 3]
```

**pop()** |删除指定索引处的项目并返回

```
x = [2, 1, 4, 3]
print(x.pop(0))  # remove and return the item at index 0# output: 2
```

如果需要返回值
`remove()`按值删除元素，使用
`del`按索引删除元素
`pop()`按索引删除元素。

# 何时使用:列表 vs 元组 vs 集

**列表**

*   python 社区中的一个强大文化是在列表中存储*同质*数据。例如:`lst= [1, 2, 3, 4, 5]`
*   可变的，这意味着你可以在数据赋值后改变列表。
*   常见操作:`append`、`extend`、`insert`、`remove`、`pop`、`reverse`、`count`、`copy`、`clear`。

**元组**

*   python 社区中的一种强势文化是使用 tuple 来存储*异构*数据。比如:`tup = (1, a, 3, d, X)`
*   不可变的，也就是说数据赋值后不能更改。
*   因为您不能添加或删除其中的项目，所以没有方法可以让 tuple 执行这类操作。元组中常见的操作:`count`和`index`。

**设定**

*   集合用于存储唯一值。它不允许任何重复元素，而 list 和 tuple 允许重复元素。例如:

```
x = set(["foo", "bar", "baz", "foo", "qux"])print(type(x))             # output: <class 'set'>
print(x)                   # output: {'qux', 'baz', 'foo', 'bar'} # another way to define set
x = {"foo", "bar", "baz", "foo", "qux"}
print(x) # output: {'bar', 'foo', 'baz', 'qux'}
```

*   它是*无序的*，而列表和元组是有序的。
*   集合本身可以被修改，但是集合中包含的元素必须是不可变的类型。例如，您可以在集合中包含一个`tuple`。

```
x = {42, "foo", (1, 2, 3), 3.14159}print(x)         # output: {42, 3.14159, 'foo', (1, 2, 3)}
```

# 集合中的常见操作

**工会()**

```
x = {"a", "b", "c", "d"}
y = {"b", "d", "a", "e"}z1 = x | y
z2 = x.union(y)print(z1)                    # output: {'d', 'e', 'c', 'b', 'a'}
print(z2)                    # output: {'d', 'e', 'c', 'b', 'a'}
```

多个集合上的并集和交集

```
a = {1, 2, 3, 4}
b = {2, 3, 4, 5}
c = {3, 4, 5, 6}
d = {4, 5, 6, 7}union = a.union(b, c, d)
intersection = a.intersection(b, c, d)print(union)                 # output: {1, 2, 3, 4, 5, 6, 7}
print(intersection)          # output: {4}
```

# 检查内存使用情况

```
import sysa, b, c, d = "abcde", "xy", 2, 15.06
print(sys.getsizeof(a))
print(sys.getsizeof(b))
print(sys.getsizeof(c))
print(sys.getsizeof(d))
```

# *args 和**kwargs

***args**

通过使用 args，我们可以向一个函数传递可变数量(任意数量)的参数。

```
def sum_lists(*args):
    return list(map(sum, zip(*args))) a = [1, 2, 3]
b = [1, 2, 3]
c = [2, 3, 4]result = sum_lists(a, b, c)
print(result)# output: [4, 7, 10]
```

*** *夸脱**

如果我们不知道作为函数参数传递的关键字参数的数量。然后，我们可以使用 kwargs。它允许函数中有可变数量的命名参数(类似于 key:value pair)。这里有一个例子，

```
def get_move_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key} = {value}")print()if __name__ == "__main__":# movie info 1get_move_info(
    name="Interstellar", 
    release_year=2014,  
    director="Christopher Nolan")# movie info 2
    get_move_info(
    name="The Machinist",
    release_year=2004,
    director="Brad Anderson",
    lead_actor="Christian Bale")
```

输出

```
name = Interstellar
release_year = 2014
director = Christopher Nolanname = The Machinist
release_year = 2004
director = Brad Anderson
lead_actor = Christian Bale
```

这是另一个例子，

```
def information(**data):
    for key, value in data.items():
        print(f"{key}: {value}") print() if __name__ == "__main__":information(
        Firstname="Sadman", 
        Lastname="Soumik", 
        Age=26, 
        Phone=1234567890
   )information(
        Firstname="John",
        Lastname="Wood",
        Email="johnwood@nomail.com",
        Country="Wakanda",
        Age=25,
        Phone=9876543210,
    )
```

输出

```
Firstname: Sadman
Lastname: Soumik
Age: 26
Phone: 1234567890Firstname: John
Lastname: Wood
Email: johnwood@nomail.com
Country: Wakanda
Age: 25
Phone: 9876543210
```

# 检查文件是否存在

```
import osif os.path.isfile(filepath):       # filepath = location of the file
   print("File exists") 
```

# 检查目录是否存在，如果不存在，创建目录

```
SAVE_DIR = "target_dir"  
if not os.path.exists(SAVE_DIR):        
    os.makedirs(SAVE_DIR)
```

# λ、贴图和过滤器

**λ**

*   Lambda 函数在 Python 中被称为匿名函数。这些类型的函数没有任何名称。
*   可以接受任意数量的参数。
*   仅限于单个表达式。
*   只用了一次。
*   当你在短时间内需要一个函数时，使用 Lambda 函数。

```
multiply = lambda x, y: x * y
print(multiply(2, 2))                # output: 4multiply = lambda x, y, z: x * y * z
print(multiply(2, 2, 2))             # output: 8
```

**地图**

```
# basic syntax
map(function_object, iterable_1, iterable_2, ...)
```

`map`采用一个函数对象，和 iterables(例如)

```
# a function that adds 10 with every number
def add_ten(x):
    return x + 10x = [1, 2, 3, 4]
ans = map(add_ten, x)
print(list(ans))# OUTPUT: [11, 12, 13, 14]
```

**过滤器**

它的工作方式类似于`map`。不同的是，它从列表中满足某个条件的元素生成一个新的列表。在参数的情况下，它需要一个函数和**一个** iterable。
基本语法:

```
filter (function, iterable)
```

下面的代码对`seq`不做任何事情，因为 lambda 函数不包含任何条件。

```
seq = [1, 2, 3, 4]
result = filter(lambda x: x + 1, seq)
print(list(result))# OUTPUT: [1, 2, 3, 4]
```

`filter`只在条件下起作用，所以我们需要用`lambda`给出一些条件语句。

```
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
even_numbers = filter(lambda x: x % 2 == 0, numbers)
print(list(even_numbers))# OUTPUT: [2, 4, 6, 8]
```

没有`lambda`函数，上述代码可以写成如下

```
def even_numbers(num):
    # return the numbers which are only divisible by 2
    return num % 2 == 0db = [1, 2, 3, 4, 5, 6, 7, 8, 9]
even_numbers = filter(even_numbers, db)
print(list(even_numbers))# OUTPUT: [2, 4, 6, 8]
```

# 集合模块

## **计算列表中不同元素的数量**

```
from collections import Counter

words = ["a", "b", "c", "a", "b", "a"]

print(dict(Counter(words)))
# {'a': 3, 'b': 2, 'c': 1}print(list(Counter(words).keys()))
# ['a', 'b', 'c']print(list(Counter(words).values()))
# [3, 2, 1]
```

## **查找列表中最频繁出现的元素**

```
from collections import Counter

def most_frequent(lst):
    data = Counter(lst)
    return data.most_common(1)  # returns most frequent 1 element

list = [2, 1, 2, 2, 1, 3, 3, 3, 2]
print(most_frequent(list)) # output: [(2, 4)]  # means, 2 is the most frequent element which appears 4 times.
```

另一个例子:

```
from collections import Counter

def most_frequent(lst):
    data = Counter(lst)
    return data.most_common(1)[0][0]                     # [0][0] is the first element of a matrix

list = [2, 1, 2, 2, 1, 3, 3, 3, 2]
print(most_frequent(list))# output: 2
```

## **使用命名元组创建类**

虽然我们大多数人都熟悉创建类和对象的简单方法，如下所示:

```
class Transaction:
    def __init__(self, amount, sender, receiver, date):
        self.amount = amount
        self.sender = sender
        self.receiver = receiver
        self.date = datetrnsaction_1 = Transaction(100, "Alice", "Bob", "2019-01-01")
print(trnsaction_1.receiver)# output: Bob
```

但是如果对象是**不可变的**，那么我们可以用集合模块中的 namedtuple 更容易地创建同样的东西。

```
from collections import namedtupleTransaction = namedtuple('Transaction', ["amount", "sender", "receiver", "date"])transaction_1 = Transaction(amount=100, sender="Alice", receiver="Bob", date="2019-01-01")print(transaction_1.receiver)
# output: Bob
```

我们还可以以字典格式打印上述信息:

```
print(transaction_1._asdict())# output: {'amount': 100, 'sender': 'Alice', 'receiver': 'Bob', 'date': '01/01/2019'}
```

## **使用集合模块中的 defaultdict 创建字典。**

```
from collections import defaultdict

employee_record = [
    ("Kabir", "ML", "level-b"),
    ("Sunehra", "SDE", "level-b"),
    ("Smith", "ML", "level-c"),
    ("William", "HR", "level-c"),
]

employee_name_by_dept = defaultdict(list)

for name, dept, level in employee_record:
    employee_name_by_dept[dept].append(name) # dept as key, name as values

print(dict(employee_name_by_dept))#output: {'ML': ['Kabir', 'Smith'], 'SDE': ['Sunehra'], 'HR': ['William']}
```

## **来自收款模块的提示**

```
from collections import deque

employee_list = ["Soumik", "Jamie", "Smith"]
employee_list_deque = deque(employee_list)

# O(1) time performance
employee_list_deque.appendleft("Sunehra")
print(list(employee_list_deque))# output: ['Sunehra', 'Soumik', 'Jamie', 'Smith']
```

尽管`deque`在序列的开头添加条目比列表更有效，但是`deque`并没有比列表更有效地执行所有的操作。例如，访问一个`deque`中的随机项具有`O(n)`性能，但是访问一个列表中的随机项具有`O(1)`性能。

当需要快速插入或删除集合中的元素时，使用`deque`。

## 从多个词典中创建词典列表

```
from collections import ChainMap

salary = {"SDE": 100000, "HR": 80000, "MTO": 60000}
office_hq = {"Asia": "Singapore", "Europe": "Dublin", "North America": "USA"}
age_limit = {"SDE": 40, "HR": 50}

employee_info = ChainMap(salary, office_hq, age_limit)
print(employee_info.maps)
```

输出:

```
[{'SDE': 100000, 'HR': 80000, 'MTO': 60000}, {'Asia': 'Singapore', 'Europe': 'Dublin', 'North America': 'USA'}, {'SDE': 40, 'HR': 50}]
```

## 使用 OrderedDict 创建有序字典

```
import collections

# remembers the order
d = collections.OrderedDict()
d["A"] = 65
d["C"] = 67
d["B"] = 66
d["D"] = 68

for key, value in d.items():
    print(key, value)
```

输出:

```
A 65
C 67
B 66
D 68
```

# 面向对象的编程概念

## 在 Python 中创建类

```
class Transaction: def __init__(self, sender, recipient, amount):
        self.sender = sender
        self.recipient = recipient
        self.amount = amount def __str__(self):
        return f"{self.sender} -> {self.recipient} : {self.amount}" if __name__ == "__main__":
    transaction_1 = Transaction("Alice", "Bob", 100)
    print(transaction_1)
```

输出:

```
Alice -> Bob : 100
```

## Python 中的继承

定义基类/父类:

```
class Employee: def __init__(self, name, salary):
        self.name = name
        self.salary = salary def __str__(self):
        return f"{self.name} earns {self.salary}" if __name__ == "__main__":
    e1 = Employee("John", 100)
    print(e1)
```

产出:

```
John earns 100
```

现在我们将定义扩展基类`Employee`的另一个类超类/子类

```
class Employee: def __init__(self, name, salary):
        self.name = name
        self.salary = salary def __str__(self):
        return f"{self.name} earns {self.salary}" class Manager(Employee): def __init__(self, name, salary, bonus):
        super().__init__(name, salary)
        self.bonus = bonus def __str__(self):
        return f"{self.name} earns {self.salary} and gets  {self.bonus} bonus" if __name__ == "__main__":
    e1 = Employee("John", 100)
    e2 = Manager("John", 100, 10)
    print(e2)
```

输出:

```
John earns 100 and gets 10 bonus
```

## Python 中的多态性

多态意味着不止一种形式，同一个对象根据需求执行不同的操作。

多态性可以通过两种方式实现，它们是

1.  方法覆盖
2.  方法重载

*方法重载*是指在同一个类中使用相同的方法名编写两个或两个以上的方法**，但传递的参数不同。**

*方法覆盖*意味着我们在不同的类中使用方法名**，这意味着父类方法在子类中使用。**

**Python 中的方法重载**

当人们在像 Java 这样的语言中进行方法重载时，他们通常想要一个默认值(如果不需要，他们通常想要一个具有不同参数的方法)。在 Python 中，我们不这样做。因此，在 Python 中，我们可以像下面这样分配默认值:

```
class Employee: def __init__(self, name, salary):
        self.name = name
        self.salary = salary def full_name(self, first_name=None, last_name=None):
        if first_name and last_name:
            return f"{first_name} {last_name}"
        else:
            return f"{self.name}" if __name__ == "__main__":
    employee = Employee("John", 10000)
    print(employee.full_name())
    print(employee.full_name("John", "Doe"))
```

输出:

```
John
John Doe
```

**Python 中的方法覆盖**

```
class Dog:
    def bark(self):
        print("WOOF") class BobyDog(Dog):
    def bark(self):
        print("WoOoOoF!")if __name__ == "__main__":
    big_dog = Dog()
    big_dog.bark() baby_dog = BobyDog()
    baby_dog.bark()
```

输出

```
WOOF
WoOoOoF!
```

**Python 中的封装**

*将*的变量和方法包装成一个实体的过程被称为**封装**。它是面向对象编程的基础概念之一。它作为一个防护罩，对直接访问变量和方法 进行 ***限制，可以防止意外或未经授权的数据修改。我们可以通过以下方式实现封装:***

```
class Human: def __init__(self):
        self.__var = "private variable"
        self.var = "public variable" def print_var(self):
        # we can access the private variable in the class
        print(self.__var) if __name__ == "__main__":
    h = Human()
    h.print_var()        # prints "private variable"

    print(h.var)         # prints "public variable"

    # private variable cannot be accessed outside the class 
    print(h.__var)       # does not work, will give error
```

作者:萨德曼·卡比尔·苏米克
领英:[斯克苏米克](https://www.linkedin.com/in/sksoumik/)