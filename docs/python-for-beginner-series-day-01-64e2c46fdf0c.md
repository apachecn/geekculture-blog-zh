# Python 初学者系列| Day-01

> 原文：<https://medium.com/geekculture/python-for-beginner-series-day-01-64e2c46fdf0c?source=collection_archive---------5----------------------->

在这里，我们将详细了解 python 的概念

![](img/b9cefbe78d1cd11eaedfe9b65b9bed5a.png)

*   在第 01 天，我们将看到以下主题

1.  打印功能
2.  输入功能
3.  变量

**简介:**

*   Python 是一种通用、通用、面向对象的高级编程语言。
*   Python 是一种动态类型和垃圾收集的编程语言。
*   它是由吉多·范·罗苏姆在 1985- 1990 年间创立的。
*   通过使用 python，我们可以进行 web 开发、机器学习、数据科学，以及枯燥的日常任务。

1.  **打印功能**

*   print()函数用于将指定的消息显示到屏幕或其他标准输出设备上。

```
print(2)
print(2+3)print("welcome to day 01 print function")
```

*   在上面的例子中，我们在 print 函数中使用双引号来显示消息
*   我们也可以在打印函数中使用单引号

```
print('I am a good python developer & automation expert')
```

*   此外，我们可以使用 print 函数的 print，即在 print 函数和另一个 print 函数中，在这种情况下，我们必须使用单引号和双引号，以防我们使用双引号或单引号，print 函数会认为所有函数都是独立的函数，因此会出现错误

```
print("print("I will complete 100 days challenge")")
                  ^
SyntaxError: invalid syntax
```

预期的输出将是

```
print("print(I will complete 100 days challenge')")
```

**2。变量:**

*   变量类似于保存一个带有名字的电话号码，以便下次引用
*   在变量声明中，我们必须关注以下几点

1.它必须以字母开头，

2.它区分大小写，

3.允许一个字母带一个数字，

4.只允许使用“_”符号&不要使用保留关键字

*   在下面的例子中，我想显示三次相同的消息，而不是输入三次我们可以使用相同的打印功能

```
print("Hello mynotes \n Hello Mynotes \n Hello Mynotes")name="Mynotesoralcedba"
print(name) 
```

*   在同一个变量名中，如果我们赋了另一个值，它是保持旧值还是新值

```
name="mynotes"
print(name)o/p:mynotesname="mynotesoracledba"print(name)o/p:mynotesoracledba
```

*   在上面的例子中，print 函数第一次将名称变量输出显示为“my notes ”,因为在 python 中“**变量总是采用最近的值”，所以我们用相同的变量指定了新值。**

**3。输入功能:**

*   它类似于 print()函数，但唯一的小区别是从用户那里获取输入并显示出来
*   为了从用户那里获得输入细节，我们使用了这个函数

```
input('What is your name?')orprint("Please enter your name")
name=input()
print("Hello",name)
```

*   在上面的例子中，我们也可以在一行中直接从用户那里获得用户名，或者我们可以对消息**“please enter your name”**使用 print 语句，我们可以将该值赋给一个变量，以便通过 print 函数显示
*   在另一个例子中，我们也可以添加两个值

```
print("Please enter value1")
a=input()
print("Please enter value2")
b=input()
print("Total is",a+b)
```

*   在另一个例子中，如何找到用户名长度细节

```
#Get the user name detailsprint("Welcome to python world" "  " + (input('Programmer name is ')))#to find name lengthprint( len(input("The programmer name is")))
```

*   这里最重要的是行注释，用于理解行/代码的使用细节

```
single line comment = #multi line comments =  """no of lines """
```