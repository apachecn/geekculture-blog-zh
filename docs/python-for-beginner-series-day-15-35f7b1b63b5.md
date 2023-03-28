# Python 初学者系列|第 15 天

> 原文：<https://medium.com/geekculture/python-for-beginner-series-day-15-35f7b1b63b5?source=collection_archive---------16----------------------->

这里我们来看看列表函数的例子

![](img/90aa6a9660cffcc1f04e9962aae964f8.png)

*   在第 15 天，今天我们将使用**列表函数做一些练习。**
*   在这里，我们要做的练习是从姓名列表中随机选择一个姓名。被选中的人必须支付每个人的食物账单。

**说明:**

*   不允许使用`choice()`功能
*   您必须输入所有的名称，名称后面跟一个逗号，然后是空格，因为我们必须将基于的字符串拆分成单个的单词，并将它们放在一个**列表**中

```
# Import the random module here
import random
# Split string method
names_string = input("Enter the names, separated by a comma. ")
names = names_string.split(", ")

print(names)
```

*   在上面的例子中，我们已经导入了随机模块，用户名使用逗号分隔值传递，即我们的示例输出存储在" **names_string** 中
*   在下一步中，通过使用 split 函数(逗号)，我们将值存储在一个列表中

**可能的输出:**

```
Enter the names, separated by a comma. Rqm, Mynotes, ORACLE, DBA

['Ram','Mynotes','ORACLE','DBA']
```

*   在同一个例子中，我将在特定的索引位置打印列表中的值

```
print(names[0])
```

**可能的输出:**

```
Ram
```

*   在这里，我们如何显示随机值在程序中使用随机模块，这是很容易显示随机值。
*   即使我们使用 random int 函数，我们也必须知道起始值和结束值。
*   从列表中我们如何得到最终值？这里我们可以使用名为“len”的函数来获取列表中的数值

```
# Import the random module here
import random
# Split string method
names_string = input("Enter the names, separated by a comma. ")
names = names_string.split(", ")

print(names) #Enter the names, separated by a comma. RAM, SAM, TAN, VAN

list_num=len(names)
print(list_num)
```

**可能的输出:**

```
['RAM', 'SAM', 'TAN', 'VAN']

4 #lenth funtion output
```

*   我们知道列表函数的索引位置从 0 开始，因此我们需要从总值中减去 1。

```
random_choice=random.randint(0,list_num-1)
print(random_choice)
```

*   我使用了上面的程序，在我的程序中添加了这两行

**可能的输出:**

```
Enter the names, separated by a comma. RAM, SAM, TAN, VAN
2
```

*   在这里，我们将值分配给一个随机选项，它创建并显示从 0 到 3 的值。
*   如果我们想知道随机索引位置名称

```
person_name=names[random_choice]
```

*   现在我们可以看到整个程序

```
# Import the random module here
import random
# Split string method
names_string = input("Enter the names, separated by a comma. ")
names = names_string.split(", ")

#get the lenght of list value
list_num=(len(names))

#get the random value 
random_choice=random.randint(0,list_num-1)
# get the random person name
person_name=names[random_choice]
print(person_name +  " Who's going to pay the meal bill ")
```

**可能的输出:**

```
Enter the names, separated by a comma. RAM, SAM, TAN, VAN
TAN Who's going to pay the meal bill
```

*   这个程序的替代方法我们可以使用 **choice()，**它将使我们使用非常少的代码，这就是为什么我们在指令中提到在得到逻辑之前不要使用这个函数。

```
# Import the random module here
import random
# Split string method
names_string = input("Enter the names, separated by a comma. ")
names = names_string.split(", ")

#random modul choice function to generate automatic range of numbers
person_name=random.choice(names)
print(person_name +  " Who's going to pay the meal bill ")
```

**可能的输出:**

```
VAN Who's going to pay the meal bill
```

*   希望你能理解这个列表函数逻辑程序，如果你不理解，请阅读以前的博客并练习这个例子