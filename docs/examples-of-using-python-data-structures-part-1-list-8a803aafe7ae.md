# 使用 Python 数据结构的例子，第 1 部分:列表

> 原文：<https://medium.com/geekculture/examples-of-using-python-data-structures-part-1-list-8a803aafe7ae?source=collection_archive---------55----------------------->

## 使用 Python List 的一些有用示例

![](img/27a1e262bf922b719e2786eab07823ab.png)

Photo by [Adeolu Eletu](https://unsplash.com/@adeolueletu?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Python 有四种内置的数据结构，即列表、字典、集合和元组。在本文中，我们将探索 python 列表。

让我们看一些使用 python 列表的常见但有用的例子。所有这些例子都是用 Python 3 写的。

# 目录

## 列表索引

Python 列表索引是[从零开始的索引](https://en.wikipedia.org/wiki/Zero-based_numbering)。其中第一个元素索引为 0，第二个元素索引为 1，依此类推…

```
>>> a_list=['first','second','third','fourth']
>>> print(a_list[0])
first
>>> print(a_list[1])
second
```

Python 也支持负索引。`-1`给出列表的最后一个元素，`-2`给出倒数第二个元素。

```
>>> print(a_list[-1])
fourth
>>> print(a_list[-2])
third
```

## 列表切片

Python 列表可以通过提到 index 来切片。

`list_name[<start_index>:<end_index>:<interval>]`列表将从`start_index`开始切片，直到`end_index`(不含)跳过`interval`步。三个(`start_index`、`end_index`、`interval`)中的任何一个都可以为空。如果`start_index`丢失，它将被设置为列表的起始索引。类似地，如果`end_index`丢失，它将被设置为列表的最后一个索引。让我们看一些例子。

```
>>> a_list[1:3]
['second', 'third']
>>> a_list[-4:-1]
['first', 'second', 'third']
>>> a_list[::2] #every second element from start
['first', 'third']
>>> a_list[1::2] #every second element starting from second
['second', 'fourth']
>>> a_list[::-1] #you can also reverse the list
['fourth', 'third', 'second', 'first']
```

您可以使用以下两种方法之一创建空列表。

```
>>> blank_list=[]
>>> type(blank_list)
<class 'list'>
>>> blank_list=list()
>>> type(blank_list)
<class 'list'>
```

## 列表理解

我们可以使用列表理解从另一个列表生成一个列表。这是一条更近的路。在下一个代码片段中，我们使用一个循环来生成一个包含`a_list`的第一个字符的列表。它还允许 python 决定实现的最佳方式(顺序或并行)。

```
>>> ac_list=[]
>>> for item in a_list:
...     ac_list.append(item[0])
... 
>>> print(ac_list)
['f', 's', 't', 'f']
```

但是如果我们使用列表理解，那么代码会是这样的

```
>>> ac_list=[item[0] for item in a_list]
>>> print(ac_list)
['f', 's', 't', 'f']
```

## 列表操作

我们可以在一个列表上执行多个操作。其中最常见的是追加、扩展、删除、弹出等。我们将在下面的代码片段中看到它们的运行。

我们可以使用 append 方法在列表的末尾添加一项。

```
>>> a_list.append('fifth') 
>>> print(a_list)
['first', 'second', 'third', 'fourth', 'fifth']
```

我们可以将一个列表中的多个元素追加到另一个列表中

```
>>> a_list.extend([‘sixth’,’seventh’])
>>> print(a_list)
[‘first’, ‘second’, ‘third’, ‘fourth’, ‘fifth’, ‘sixth’, ‘seventh’]
```

注意:如果我们试图使用 append 方法添加一个列表，那么整个列表被认为是一个条目并被插入到列表中。

```
>>> a_list.append([‘sixth’,’seventh’])
>>> print(a_list) 
[‘first’, ‘second’, ‘third’, ‘fourth’, ‘fifth’, ‘sixth’, ‘seventh’, [‘sixth’, ‘seventh’]]
```

我们可以使用 pop 方法从列表中删除项目。如果没有任何参数，它将移除最后一个元素并返回该元素。我们还可以通过将项目索引作为 pop 方法的参数传递来删除项目

```
>>> a_list.pop()
[‘sixth’, ‘seventh’]
>>> print(a_list)
[‘first’, ‘second’, ‘third’, ‘fourth’, ‘fifth’, ‘sixth’, ‘seventh’]
>>> a_list.pop(3)
‘fourth’
>>> print(a_list)
[‘first’, ‘second’, ‘third’, ‘fifth’, ‘sixth’, ‘seventh’]
```

通过使用 insert 方法引用索引，也可以将元素插入到数组中

```
>>> a_list.insert(3,’first’) 
>>> print(a_list)
[‘first’, ‘second’, ‘third’, ‘first’, ‘fifth’, ‘sixth’, ‘seventh’]
```

我们可以得到一个条目在列表中出现的次数

```
>>> print(a_list.count(‘first’))
2
```

我们也可以使用 remove 方法移除元素。在多次出现的情况下，它将删除第一次遇到的项目。

```
>>> a_list.remove(‘first’) 
>>> print(a_list)
[‘second’, ‘third’, ‘first’, ‘fifth’, ‘sixth’, ‘seventh’]
```

为了找到一个条目在列表中的存在和位置，我们可以使用列表的索引方法。如果一个项目存在，它返回该项目的索引，否则抛出`ValueError`。

```
>>> a_list.index('fifth')
3
>>> a_list.index('fourth')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: 'fourth' is not in list
```

我们还可以调用列表的排序和反转。

```
>>> a_list.sort()
>>> print(a_list)
['fifth', 'first', 'second', 'seventh', 'sixth', 'third']
>>> a_list.reverse()
>>> print(a_list)
['third', 'sixth', 'seventh', 'second', 'first', 'fifth']
```

如果我们想把一个列表复制到另一个列表。我们可以简单地使用赋值操作符(`=`)来实现。但是它会导致一些问题，我们将在下一个例子中看到。

```
>>> b_list=a_list
>>> print(b_list)
['third', 'sixth', 'seventh', 'second', 'first', 'fifth']
>>> b_list.append('seventh') # let's make some changes to the new list
>>> print(b_list)
['third', 'sixth', 'seventh', 'second', 'first', 'fifth', 'seventh']
>>> print(a_list) # The changes happened in the original list as well
['third', 'sixth', 'seventh', 'second', 'first', 'fifth', 'seventh']
```

我们可以使用复制方法来解决这个问题。

```
>>> b_list=a_list.copy()
>>> print(b_list)
['third', 'sixth', 'seventh', 'second', 'first', 'fifth', 'seventh']
>>> b_list.append('tenth') # let's make some changes to the new list
>>> print(b_list)
['third', 'sixth', 'seventh', 'second', 'first', 'fifth', 'seventh', 'tenth']
>>> print(a_list) 
['third', 'sixth', 'seventh', 'second', 'first', 'fifth', 'seventh']
```

赋值操作符创建一个浅层副本，其中第二个列表指向原始列表。而 copy 方法创建一个包含原始列表元素的完整列表。

我们可以使用 clear 方法来清除列表。

```
>>> a_list.clear()
>>> print(a_list) 
[]
```

## 列表上的其他操作

获取列表的长度。

```
>>> n_list=[1,5,3,4,2]
>>> print(len(n_list))
5
```

获取列表的最大值和最小值。

```
>>> print(min(n_list))
1
>>> print(max(n_list))
5
```

我们也可以对列表进行排序。不影响原列表。

```
>>> print(sorted(n_list))
[1, 2, 3, 4, 5]
>>> print(n_list) 
[1, 5, 3, 4, 2]
```

这是使用列表及其方法的一些基本示例。

希望你喜欢。敬请关注更多内容。