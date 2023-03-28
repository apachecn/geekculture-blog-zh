# 你应该每天使用的 10 个 Python 片段

> 原文：<https://medium.com/geekculture/10-python-snippets-that-you-should-use-every-day-6c1f076fd341?source=collection_archive---------4----------------------->

## №8 将使你成为专业的 python 程序员

![](img/2fd67e251625e019c7964c983d0fdd06.png)

Credits: Wayhomestudio at Freepik.com

如果你每天都在使用 Python，并且想学习一些技巧让你的代码更具可读性，那么你可以看看下面的 18 个片段。

# **1。插值格式字符串**

f 字符串解决了 C 风格格式字符串的最大问题，并且非常强大，因为它们允许任意的 Python 表达式直接嵌入到格式说明符中。

```
val = 108
print(f'Value: {val}')
```

# **2。偏好多重作业解包过度索引**

可读性是 Python 的主要优点之一，它有一个特殊的语法叫做解包，用于在一个语句中分配多个值。

```
dict = {'apple' " 120, 'banana': 60, 'nuts': 102}
items = tuple(dict.items())
print(items)
apple, banana, nuts = items #**unpacking**
```

# 3.Python 中的三元运算符

阅读起来过于复杂的单行表达式可以很容易地用 Python 编写。If/Else 表达式为使用布尔运算符和 in 表达式提供了一种可读性更好的替代方法。

```
**syntax : value if true else false**
```

# 4.列举的是新的*系列*

如果你想迭代一个 iterable，也想获取一个迭代器，你可以使用 enumerate()函数。您可以提供第二个参数来枚举，以指定开始计数的数字(零:默认值)

```
l = ['deku', 'bakugo', 'todoroki', 'naruto']
for indx, val in enumerate(l):
  print(f'Value at index {indx} is: {val}')
```

# 5.使用 *zip* 来并行处理迭代器

当迭代多个可迭代对象时，整个循环语句的问题是视觉上的噪音。使用*枚举*稍微改善了这一点，但仍然不理想。拉链前来救援。

```
agents = ['Reyna', 'Jett', 'Viper']
kills = [13, 2, 14]
for agent, kill in zip(agents, kills):
  print(f'Agent {agent} : Kills {kill}')
```

# 6.使用新的 walrus 运算符

很多时候，我们程序员看到这种获取一个值，检查它是否非零，然后使用它的模式。这使得编写程序时很难保持 Python 的核心特性即可读性。因此，在 Python 3.8 中，增加了 walrus 操作符来简化这种类型的代码。

```
if killCount := mostKills('Jett', 0)
   killFeed(killCount)
else:
   print('Jett noob')
```

# 7.知道如何分割你的序列

在蛋糕上切片和 Python 中的切片语法符合它们的本性。切片允许您以最小的努力访问序列项的子集。

```
l = [1, 2, 3, 4, 5, 6]
syntax : l[start:end]l[:] # all elements
l[:3] # start at 0 and end at 3rd element
l[3:] # start from 3rd element and end at lastuse negative slicing
l[:-1] # all elements except 6 (ie last)
l[2:-1] # start from 2nd element and end and remove last element
```

# 8.知道何时使用切片和索引

你可以依靠索引和切片，但是很多时候你会不知道 iterable 的大小，那时候你会遇到一个运行时错误。解决这个问题的最好方法是使用带星号的表达式。

```
first, *middleIDK, last = [1, 2, 3, 4, 5]first # 1
middleIDK # 2, 3, 4
last # 5
```

# 9.将任何类转换成字典

类有一个内置的属性 __dict__ 这意味着创建的对象是字典，需要时可以像字典一样使用。

```
class Obj:
 def __init__(self, a = 'test', b = 108):
    self.a = a
    self.b = b
obj = Obj()
print(vars(obj)) # vars() is pythonic way to convert to dictionary
```

# 10.使用 get 处理缺少的字典键

如果你通常使用 in 来检查一个关键字是否在字典中，那么你就错过了一个更好的选择。

您可以使用 *setdefault* 方法来获取一个键值，如果它不存在，该方法会将该键值赋给所提供的默认值。

```
agents = leaderboard.setdefault(key, [])
```

您最好不要使用它，因为它会为每个找不到的键创建一个默认值，并且会导致性能开销。

然而，如果你想要一个可读性更好的方法，你可以使用 *get* 方法和一个 walrus 操作符，就像这样。

```
if (agents := ) is None:
   leaderboad[key] = agents = [] 
```

> 如果您喜欢这篇文章，并且想了解更多关于 Python 的内容，请查看这些文章。

[](/codex/5-advanced-automation-scripts-in-python-7dc0ff845099) [## 5 个高级 Python 脚本，像专业人员一样实现自动化！

### 这些都是生活窍门！

medium.com](/codex/5-advanced-automation-scripts-in-python-7dc0ff845099) [](/codex/5-advanced-code-snippets-for-your-python-programs-3377baa3d544) [## Python 程序的 5 个高级代码片段

### 使用它们，让人们惊奇

medium.com](/codex/5-advanced-code-snippets-for-your-python-programs-3377baa3d544) [](https://python.plainenglish.io/python-functions-that-make-life-easier-9d0c41b986a8) [## 让生活更简单的 5 个 Python 函数

### 让生活更简单的 Python 函数列表。

python .平原英语. io](https://python.plainenglish.io/python-functions-that-make-life-easier-9d0c41b986a8) [](https://python.plainenglish.io/if-you-are-using-assert-you-must-read-this-7256e46dc816) [## 停止使用 Assert 进行数据验证

### 对于那些使用断言操作进行数据验证的人来说，这是一篇必读的文章。

python .平原英语. io](https://python.plainenglish.io/if-you-are-using-assert-you-must-read-this-7256e46dc816) 

你可以在我的个人资料中找到更多这样的内容。

[](/@rahulbhatt1899) [## 拉胡尔·布哈特—中等

### 阅读媒体上的拉胡尔·布哈特作品。我写 Python 脚本和文章。GitHub…

medium.com](/@rahulbhatt1899)