# Python 初学者系列| Day-02

> 原文：<https://medium.com/geekculture/python-for-beginner-day-02-156ba0293db7?source=collection_archive---------6----------------------->

在这里，我们将理解数据类型的概念

![](img/29ecff20a7a3363323fb757c16a012b4.png)

*   在第 02 天，我们将详细讨论数据类型

1.  **数据类型:**

*   这是一个重要的概念
*   变量可以存储不同类型的数据，不同的类型可以做不同的事情。
*   最重要的数据类型有

1.  int——这是一种没有任何小数的整数的编程术语

```
a =2
print(a)
```

2.浮点数—包含小数点的数字称为浮点数

```
b = 4.5
print(b)
```

3.字符串——我们知道它只是一串字符，也是用双引号或单引号定义的值。

```
c = "mynotesoracledba"
print(c)
```

4.布尔型—包含两个可能的值“真”或“假”

```
d = False
print(d)
```

*   在上面的例子中，我们可以使用“**类型**函数来标识值的数据类型

```
a = 12
b = 4.5
c = "mynotesoracldba"
d = Falseprint(type(a))
print(type(b))
print(type(c))
print(type(d))results:<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

*   在下面的例子中，我们将看到获得两位数的值，并显示该值相加的结果，即输入= 17，结果= 8(1+7)。

```
# Get the value as inputtwo_digit_number = input("Type a two digit number:  ") #17#here **"hello"[0] - h** indicates the string position indicationfirst_digit =  two_digit_number[0] #1 second_digit = two_digit_number[1] #7print(first_digit) #1print(second_digit) #7result = first_digit + second_digit#result = 17 (+ will do string concate)print(type(first_digit))#<class 'str'>print(type(second_digit))#<class 'str'>
```

*   在我们对数据类型进行更改之前，它仍然可以被视为“字符串”,而“+”符号将进行连接
*   因为我们需要 8 的预期结果，所以我们可以在变量前面添加 int 数据类型

```
result = int(first_digit) + int (second_digit)print(result)#result=8 (1+7)
```

*   在变量前面添加了数据类型后，它将作为一个整数，所以它添加了 first_digit 和 second_digit 变量。