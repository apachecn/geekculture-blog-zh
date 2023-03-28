# Python 初学者系列| Day-04

> 原文：<https://medium.com/geekculture/python-for-beginner-series-day-04-a9f970439acd?source=collection_archive---------7----------------------->

在这里，我们将理解运算符的概念&数字操作

![](img/6cd3d11e85d48d45866551d680259fc7.png)

*   在第 03 天，我们将详细讨论**运算符&数字操作**。

**操作员:**

*   它用于对变量执行操作

1.  **算术运算符:**

*   它与数值一起用于执行常见的数学运算。

```
a=1
b=3
print(a+b) #addition
print(a-b) #subtract
print(a*b) #Multiplication
print(a/b)  #division
print(a%b) #modulus mod
print(b**3) #power
```

*   在上面的例子中，我们已经详细地展示了操作和符号，我们可能会有疑问，通常我们使用乘法和“x”符号，但是这里我们使用星号代替 x 符号
*   另外我们始终要记住一件事，这里的**除法**结果总是会被**浮点**。
*   “ ****** ”符号用于表示幂，或者当您想要将数字提升到幂时使用

**2。赋值运算符:**

*   它们用于给变量赋值

```
sal = 0sal = sal + 1print(sal)
```

*   在上面的例子中，我们在每次迭代中增加 1 的值，这看起来很笨拙，但我们可以像下面的例子一样使用它。

```
a=10
b=20
#a=a+2
#a+=2
a+=1
a-=1
a*=1 #a=a*1
a/=1
a%=1
a**=2  #a=a**2
b**=2 #b=b**2
print(b)
```

**3。操作员优先级:**

*   在这里，它遵循 PEMDAS 的优先规则
*   PEMDAS 代表“**括号指数乘除加法减法**
*   当我们进行数学计算时，运算符优先级

```
print(3 * 3 + 3 / 3 - 3)
```

*   在本例中，正如我们所讨论的，它将赋予括号优先级，然后乘法和除法具有优先级，因此这里遵循 LR，即从左到右的规则乘法将是 9，除法是 1.0，最后，它减去 3，因此结果将是 7.0
*   在上面同样的例子中，我想得到结果 3.0，我们应该修改代码以得到预期的结果，也就是说，只需添加括号

```
print(3 * (3 + 3) / 3 - 3 )
```

**4。关系运算符:**

*   它们用于比较两个值

```
a=1
b=2
print(a<b)
print(a<=b)
print(a>b)
print(a>=b)
print(a==b)
print(a != b )
```

*   在本例中，我们为两个值分配不同的变量，并用不同的关系运算符进行验证。

**5。逻辑运算符:**

*   它们用于组合条件语句

```
a=True
b=False
print(a and b)
print(a or b)
print(not(a))
print(not(b ))
```

*   这里我们看到了逻辑运算符的例子

**6。按位运算符**

*   它用于比较(二进制)数。

```
a=5
b=3
print(a&b)
print(a|b)
print(a^b)a=True
b=False
print(a&b)
print(a | b)
print(a ^ b)
```

**2。数字操作:**

**1。回合:**

*   在下面的例子中，我们将把两个变量分开，结果提供了 float 的结果，如果我们需要一个舍入值，我们可以使用 int

```
a= 8 
b= 3print(type(a/b))#convert float into intprint(int(8/3))
```

*   python 提供了舍入函数来舍入值

```
print(round (8 / 3 ))#which provides the nearer whole number
```

*   万一我们要舍入具体位置的舍入值也是可能的

```
print( round (8 / 3 ,2))
```

**2。楼层:**

*   每当我们用一个数除以另一个数时，结果总是变成一个浮点数，但是 floor 会把浮点数转换成 int

```
print(8/3) #which provides the result of 2.6print(8//3) #which provides the result of 2 nearer interger value
```