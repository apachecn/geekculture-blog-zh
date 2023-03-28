# 揭开 Python 字典的神秘面纱

> 原文：<https://medium.com/geekculture/demystifying-python-dictionaries-91dc98bca87e?source=collection_archive---------15----------------------->

![](img/cce11566099ef6040c6f7efc095a95ab.png)

Photo by [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你曾经在 Python 中有过某种需要字典参与的任务，那么你可能已经在 Google 上找到了很多信息。我知道这一点，因为我也曾和你处于同样的位置。问题是你很少能找到为什么你应该以这样或那样的方式使用字典的详细解释，所以我决定把所有的部分放在这里，并展示在 Python 中使用字典时你可以有哪些选择，以及如何更好地理解它们。

# Python 中的字典是什么？

简单明了地说，字典是一种数据结构，它由**键:值**对**和**组成，要求字典中的每个键都必须是唯一的。字典中不可能有两个不同的条目具有相同的键。这些**键:值**对必须用逗号分隔。你得用大括号 **{}** 。例如，要创建第一个空字典，您可以编写:

```
my_first_dict = {}
print(my_first_dict)
print(type(my_first_dict))
```

**输出:**

```
{}
<class 'dict'>
```

如您所见，如果我们打印出字典的类型，您会看到这个用空括号 **{}** 创建的对象属于 **dict** 类。这意味着我们将有很多方法可以在字典中使用，但是以后会有更多。

不管怎样，你也可以使用 **dict** 构造函数创建一个字典:

```
my_second_dict = dict()
print(my_second_dict)
print(type(my_second_dict))
```

**输出:**

```
{}
<class 'dict'>
```

如您所见，输出与使用方括号 **{}** 创建字典是一样的。从功能的角度来看，没有。两个字典的行为是一样的，但是方括号 **{}** 的方法更快。问题是为什么？要回答这个问题，你需要从 **dis** 模块中获得一些帮助。它是 Python 字节码的反汇编器。官方文档可在以下官方网站上找到:

 [## Python 字节码的反汇编器——Python 3 . 9 . 7 文档

### 该模块通过反汇编来支持对 CPython 的分析。这个模块将 CPython 字节码作为…

docs.python.org](https://docs.python.org/3/library/dis.html) 

现在我不会深入讨论 dis 模块，因为这值得在另一篇文章中讨论。现在，我们已经知道 **dis** 模块有一个分析功能，也叫做 **dis** ，所以要使用它，我们可以定义如下:

```
from dis import dis
```

现在我们可以使用这个 **dis** 函数并传递一串将被反编译的源代码。所以让我们将它与 **{}** 和 **dist()一起使用。为了更好的可视化，我添加了一个空的 print 语句。**

```
dis("{}")
print("")
dis("dict()")
```

**输出:**

```
1           0 BUILD_MAP                0
            2 RETURN_VALUE1           0 LOAD_NAME                0 (dict)
            2 CALL_FUNCTION            0
            4 RETURN_VALUE
```

现在我们在上面看到了什么？让我们从左边的数字开始，它在两种情况下都是 **1** 。这是被分析代码的行号。因为我们的代码只有一行，所以显示的是 1。如果你反编译一个函数或者有更多行的东西，你会看到每一行产生更多的数字和指令。

现在可以看到，在 python 中调用 **{}** 只生成了两条指令: ***BUILD_MAP*** 和 ***RETURN_VALUE。*** 什么 ***BUILD_MAP*** 指令它基本上会从堆栈中存储的元素构建一个字典对象。然后 ***RETURN_VALUE*** 会简单的将结果返回给函数的调用者。所以这只是两条指令。现在，在使用 **dict()** 的情况下我们可以看到第一条指令是 ***LOAD_NAME。*** 该指令将传递给 ***dict()*** 构造函数的参数推送到堆栈上。接下来，***CALL _ FUNCTION***指令将弹出所有被推入堆栈的值，然后它将使用这些值作为自变量，并将调用可调用对象。结果将被推回堆栈。请注意，从 Python 3.6 版本开始，此指令仅用于带有位置参数的调用。最后一条指令是 ***RETURN_VALUE*** ，我们已经讨论过了。所以，如你所见，使用 ***dict()*** 构造函数创建字典会反编译 intro 三条指令， ***{}*** 会反编译成两条。

现在，您可以使用相同的方法通过传递一个 ***键:值*** 对来尝试:

```
dis("{\"One\":1}")
print("")
dis("dict(One=1)")
print("")
```

**输出:**

```
1           0 LOAD_CONST               0 ('One')
            2 LOAD_CONST               1 (1)
            4 BUILD_MAP                1
            6 RETURN_VALUE1           0 LOAD_NAME                0 (dict)
            2 LOAD_CONST               0 (1)
            4 LOAD_CONST               1 (('One',))
            6 CALL_FUNCTION_KW         1
            8 RETURN_VALUE
```

现在你大概看到了，几乎所有的指令都是一样的，除了 ***LOAD_CONST*** 指令是为了将参数压入堆栈而生成的。还有，生成***CALL _ FUNCTION _ KW***而不是 ***CALL_FUNCTION。*** 当关键字参数作为而不是位置参数传递给对象时，生成该指令。无论如何，你可以看到用 ***{}*** 语法创建字典总是比使用 ***dict()少编译成一条指令。***

现在我不确定在 python 中是否有可能测量单独指令的执行速度(如果你知道如何做，请在评论中分享)，但肯定我们可以使用 python 中的 ***timeit*** 模块来测量各种函数或一段源代码的执行时间。我也不会深入这个模块，因为这不是本文的范围，但我可以说，这个模块有 ***timeit*** 函数 ***，*** ，您可以向其传递要测量的源代码和一个计数器，它将显示代码片段将被执行多少次。首先，让我们从 ***timeit*** 模块中导入 ***timeit*** 函数

```
from timeit import timeit
```

那么，让我们用 ***{}*** 和 ***dict()*** 方法创建一个字典 1000 万次。

```
print(timeit("{}", number=10**7))
print(timeit("dict()", number=10**7))
```

**输出:**

```
0.5059882
1.6348104
```

这显示了用这两种方式创建字典一千万次需要多少秒。可以看到， ***{}*** 方法大约快三倍。当然，请记住，这将因机器而异，您将在这里看到不同的结果(即使您运行此代码 10 次，您可能也不会得到相同的结果，因为这取决于您的 CPU 负载。但总的来说，你应该能看到明显的区别。

所以现在我们知道，使用 ***{}*** 语法创建字典比使用 ***dict()*** 更快。那么我们是否应该停止使用 ***dict()*** ？如果你的演唱会是速度和速度只有那么我会说是，但使用 ***dict()*** 你可以实现一些事情更容易，使代码看起来更干净。例如，您可以拥有一个由带有 ***键、值*** 对的列表组成的列表，如下例所示:

```
sample_list = [["One", 1], ["Four", 4], ["Six", 6]]
```

然后，您可以简单地将这个列表传递给***dict()***construct，它将为您构建一个字典:

```
my_dict_v1 = dict(sample_list)
print(my_dict_v1)
```

**输出:**

```
{'One': 1, 'Four': 4, 'Six': 6}
```

现在，如果您想使用 ***{}*** 构造来做同样的事情，您将不得不遍历 list 对象并将新的键及其值添加到空字典中:

```
sample_list = [["One", 1], ["Four", 4], ["Six", 6]]my_dict_v2 = {}
for key, value in sample_list:
    my_dict_v2[key] = value

print(my_dict_v2)
```

***输出:***

```
{'One': 1, 'Four': 4, 'Six': 6}
```

当然，如果你了解字典理解，你可以这样做:

```
my_dict_v3 = {key: value for (key, value) in sample_list}
print(my_dict_v3)
```

**输出:**

```
{'One': 1, 'Four': 4, 'Six': 6}
```

你可以看到这三种情况下的输出是相同的，但我个人认为 ***dict()*** 方法最容易阅读和理解，因为在看到一个单词 **dict** 后，我立即知道我们要在这里创建一个 dictionary 对象。其他构造需要通读代码，看看发生了什么。当然，这只是我的个人观点，但我的目标不是说哪种方法是最好的，而是展示在使用字典时你有哪些选择，以便你可以决定哪种选择更适合你。顺便说一下，如果你不理解字典理解的例子，不要担心。只要稍加练习，这是有意义的。

无论如何，关于使用 ***dict()*** 构造函数创建字典，我还想分享一件事——你可以将关键字参数传递给构造函数，它会创建一个字典:

```
kwargs_dict = dict(position=4, name="Peter", Color="Blue", id=25663)
print(kwargs_dict)
print(type(kwargs_dict))
```

**输出:**

```
{'position': 4, 'name': 'Peter', 'Color': 'Blue', 'id': 25663}
<class 'dict'>
```

使用字典的 ***{}*** 构造无法做到这一点。

# 字典方法

在这一节中，我想快速回顾一下使用词典时需要用到的一些最常见的方法。首先，让我们创建一个示例字典。为了简单起见，让我们使用与上一个例子中相同的字典，就叫它 example_dict:

```
example_dict = dict(position=4, name="Peter", Color="Blue", id=25663)
```

那么有了这本字典，你可以用什么方法呢？最常见的事情之一是知道你的字典的所有键。您可以通过使用 ***keys()*** 方法来实现:

```
print(example_dict.keys())
```

**输出:**

```
dict_keys(['position', 'name', 'Color', 'id'])
```

为什么需要字典键之类的东西呢？好吧，也许你想遍历你的字典并通过键访问它的值:

```
for key in example_dict.keys():
    print(f"key:{key}, value:{example_dict[key]}")
```

**输出:**

```
key:position, value:4
key:name, value:Peter
key:Color, value:Blue
key:id, value:25663
```

另一件很常见的事情是获取字典中的所有值。可以通过调用 ***values()*** 方法来实现。当您对字典值感兴趣，但对键不感兴趣时，可以使用这个方法。例如，假设您有一个字典，其中的键是某种问题跟踪系统中的票证 id，值是程序员或其他任何人在该问题上花费的小时数。因此，如果您想知道在这些问题上花费了多少时间(忽略问题本身)，您可以使用 ***values()*** 方法。

```
example_dict_v1 = {"Ticket-1": 8, "Ticket-2": 20, "Ticket-3": 36}
print(example_dict_v1.values())

for value in example_dict_v1.values():
    print(f"Hours spent:{value}")
```

**输出:**

```
dict_values([8, 20, 36])
Hours spent:8
Hours spent:20
Hours spent:36
```

使用字典的另一个非常有用的方法是 **get()** 。您必须将一个键名传递给 **get()** 方法，它返回与该键相关的值，但是这个方法的好处是，如果字典中不存在该键，那么它返回 ***None*** 。所以举个例子，如果我们有和上例一样的字典 ***example_dict_v1，*** 那么下面的尝试会导致异常(因为***【Ticket-6】***键在字典中不存在):

```
hours_on_issue = example_dict_v1["Ticket-6"]
```

**输出:**

```
Traceback (most recent call last):
  File "C:/python_projects/dictionaries_review/main.py", line 70, in <module>
    hours_on_issue = example_dict_v1["Ticket-6"]
KeyError: 'Ticket-6'
```

这不是一件好事，因为它会导致你的程序崩溃。因此，为了避免这种情况，您必须添加 try/catch 块来处理任何意外的异常，但是我们实际上并不需要这样做，因为我们已经有了*方法——如果关键字不在字典中，它就简单地返回 None。*

```
*hours_on_issue = example_dict_v1.get("Ticket-6")
print(hours_on_issue)*
```

***输出:***

```
*None*
```

*现在，如果你想用新值更新我们的字典，该怎么做呢？例如，添加一个新的票证问题及其花费的时间？嗯，我们有*更新() ***:*** 的方法**

```
**example_dict_v1.update({"Ticket-13": 48})
example_dict_v1.update(Ticket16=28)
print(example_dict_v1)**
```

****输出:****

```
**{'Ticket-1': 8, 'Ticket-2': 20, 'Ticket-3': 36, 'Ticket-13': 48, 'Ticket16': 28}**
```

**请注意，我在这里展示了两种方式:您可以添加新元素，就像添加另一个带有 ***{}*** 构造的字典一样，或者您可以通过关键字参数语法添加带有值的新键。无论如何，您可以看到我们的字典已经用新的键成功更新了。**

**我窟讨论的最后两个方法是*copy()和 ***pop()。*** 我认为在一个例子中谈论它们是一个好主意，因为如果你将你的字典分配给另一个字典而没有 ***copy()*** 并对其执行操作，就有可能产生一些相当严重的错误。所以，让我们假设你想把你当前的字典分配给另一个对象，并从中删除一些键。因此，您可以通过使用 ***pop()*** 方法并提供您想要移除的键来移除任何 ***key:value*** 对。但是这里有一个问题——您的新对象实际上会保存对原始 dictionary 对象的引用，因此通过对新对象进行操作，您也将对原始对象执行操作。考虑下面的例子:***

```
**example_dict_v1_copy = example_dict_v1
print(hex(id(example_dict_v1)))
print(hex(id(example_dict_v1_copy)))
example_dict_v1_copy.pop("Ticket-13")
print(example_dict_v1)
print(example_dict_v1_copy)**
```

****输出:****

```
**0x101a4e0
0x101a4e0
{'Ticket-1': 8, 'Ticket-2': 20, 'Ticket-3': 36, 'Ticket16': 28}
{'Ticket-1': 8, 'Ticket-2': 20, 'Ticket-3': 36, 'Ticket16': 28}**
```

**正如你所看到的，我想在不影响原有内容的情况下，从我的名为***example _ dict _ v1 _ copy***的新字典中删除一个键，但事实并非如此。你可以看到 ***Ticket-13*** 键从两个字典中都被删除了。正如我所说，这是因为您的新 dictionary 对象只是对原始对象的引用(指向原始 dictionary 所在的内存位置)。通过使用 ***id()*** 函数打印我们的对象在内存中驻留的地址来证明这一点。这就是 ***copy()*** 方法的用武之地。如果您在将它分配给新字典对象时将其用于原始字典对象，将在内存中创建一个新对象，并且在操作新字典时不会影响原始字典:**

```
**example_dict_v1_true_copy = example_dict_v1.copy()
print(hex(id(example_dict_v1)))
print(hex(id(example_dict_v1_true_copy)))
example_dict_v1_true_copy.pop("Ticket-1")
print(example_dict_v1)
print(example_dict_v1_true_copy)**
```

**输出:**

```
**0x101a4e0
0x101a4b0
{'Ticket-1': 8, 'Ticket-2': 20, 'Ticket-3': 36, 'Ticket16': 28}
{'Ticket-2': 20, 'Ticket-3': 36, 'Ticket16': 28}**
```

**如您所见，这一次原始字典没有受到影响，它们现在是不同的对象，您实际上可以看到，通过再次使用 ***id()*** 函数打印对象驻留在内存中的位置。**

**最后我想说的是，这与字典无关，但它可以帮助你检查 Python 中的任何对象。所以 Python 内置了名为 ***的函数 help()*** 。您可以将任何想要的对象传递给这个函数，Python 将显示它所知道的关于所请求对象的所有信息。您可以在此了解更多信息:**

 **[## 内置函数- Python 3.9.7 文档

### Python 解释器内置了许多始终可用的函数和类型。它们被列出…

docs.python.org](https://docs.python.org/3/library/functions.html#help)** 

**例如，如果你想了解更多关于字典的知识，你可以这样写:**

```
**help(dict)**
```

****输出:****

```
**Help on class dict in module builtins:class dict(object)
 |  dict() -> new empty dictionary
 |  dict(mapping) -> new dictionary initialized from a mapping object's
 |      (key, value) pairs
 |  dict(iterable) -> new dictionary initialized as if via:
 |      d = {}
 |      for k, v in iterable:
 |          d[k] = v
 |  dict(**kwargs) -> new dictionary initialized with the name=value pairs
 |      in the keyword argument list.  For example:  dict(one=1, two=2)
 |  
 |  Methods defined here:....**
```

**我不打算在这里添加完整的输出，因为它很长，但我希望你得到的想法。**

# **结论**

**在 Python 中使用字典可能有点棘手。我希望本文不仅能帮助您理解字典，还能帮助您探索其他 python 对象并更快地了解它们。**

**所有示例代码都位于我的 github 库下面:[https://github.com/vycart/python_dictionaries](https://github.com/vycart/python_dictionaries)**

**如果你有任何问题，我很乐意回答。**

**感谢您的阅读！**