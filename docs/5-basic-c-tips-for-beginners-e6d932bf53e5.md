# 给初学者的 5 个基本 C#技巧

> 原文：<https://medium.com/geekculture/5-basic-c-tips-for-beginners-e6d932bf53e5?source=collection_archive---------32----------------------->

## 大家好！我决定加入一些不太为人所知、但能帮助提高生产率的技巧。

![](img/cd1e50e7d28c20c2cc239c8362f4fb3b.png)

Photo by [Amol Tyagi](https://unsplash.com/@amoltyagi2?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 1.可空类型

有时，变量必须包含空值。在使用对象关系映射时，这种情况会更频繁地发生，因为在大多数数据库中，我们总是有一些表，这些表的列可以包含空值。要使普通变量成为接受 null 的变量，只需添加一个“？”以该类型的结尾为例:

```
int? i = null;
double? D = null;
```

## 2.三元运算符？:'

计算布尔表达式并返回两个表达式之一的结果，条件运算符的语法如下:

```
condition ? consequent : alternative
```

基本上，**三元运算符**在单行代码中取代了传统的 if 块。实际上它是这样工作的，想象下面的代码块:

```
**int** x = 150;
**int** y = 456;
**int** maximum;**if**(x > y)
  maximum = x;
**else** maximum = y;
```

让我们看看前面使用三元运算符的例子:

```
**int** x = 150;
**int** y = 456;
**int** maximum = (x > y) ? x : y;
```

## 3.零合并操作符？?'

零合并操作符`??`返回其左边操作数的值，如果它不是`null`；否则，它计算右边的操作数并返回结果。如果左边的操作数计算结果为非空，那么`??`操作符不会计算右边的操作数。

让我们考虑下面的代码:

```
**object** a= **null**;**object** b= **new** **object**();**object** c = (a != **null**) ? a : b;
```

然而，使用 **null-coalesce 操作符**，我们能够进一步缩短代码(如果左边的对象为 null，那么它将返回右边的值)。

```
**object** a = **null**;
**object** b = **new** **object**();
**object** c = a ?? b;
```

## **4。类型推断**

C#是一种强类型语言，默认的类型声明是显式类型。例如，下面的代码片段声明并初始化了两个变量。第一个变量 *myString* 是一个字符串类型变量，第二个变量 *age* 是一个整数类型变量。

```
**string** myString = "Hello World";
**int** age = 25;
```

类型推理允许我们停止编写传统的代码，我们可以这样写:

```
**var** myString = "Hello World";
**var** age = 25;
```

即使有了类型的推理 **C#** 它仍然是一种**强类型语言**并且一旦推理完成(即变量接收了一个值),它的内容类型就永远不能改变。再说一遍，这只是在编程层面。当编译器开始工作时，它根据需要分配正确的类型，省去了我们修饰各种类型和方法回调的麻烦。

## 5.字符串。IsNullOrEmpty()

一个非常有用的 String 类的静态方法。测试字符串是 null(空)还是空("")的强大工具。下面是一个代码示例:

```
**if** (**String**.IsNullOrEmpty(someStringVariable))
**{
**   **return** "The variable is null or empty";
**}
else
{
   return** "The variable contains text";
**}**
```

# 其他文章

[](https://levelup.gitconnected.com/asp-net-5-authorization-and-authentication-with-bearer-and-jwt-2d0cef85dc5d) [## ASP。NET 5:使用无记名和 JWT 进行授权和认证

### 本文的目的是展示授权、无记名身份验证和 JWT (JSON Web Token)是如何在

levelup.gitconnected.com](https://levelup.gitconnected.com/asp-net-5-authorization-and-authentication-with-bearer-and-jwt-2d0cef85dc5d) [](https://levelup.gitconnected.com/asp-net-core-user-secrets-2964219f675b) [## ASP。网络核心-用户秘密

### 本文解释了在 ASP.NET 核心开发过程中存储和检索敏感数据的技术

levelup.gitconnected.com](https://levelup.gitconnected.com/asp-net-core-user-secrets-2964219f675b)