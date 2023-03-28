# 什么是面向对象编程中的对象？

> 原文：<https://medium.com/geekculture/what-is-an-object-in-object-oriented-programming-a97b8088dcce?source=collection_archive---------8----------------------->

初学者的高水平概述。

![](img/d7669b0e2e83dbf44f9bb32b509089ab.png)

What is OOP?

这是一个面向计算机科学和编程初学者(也可能是那些从未编程的人)的解释。我将对一些事物进行广泛而简单的描述。这有一个不幸的副产品，就是遗漏了一些规则和定义的麻烦和例外。

## 什么是数据结构？

在我们放大一个物体之前，我们必须先缩小它。

数据结构是存储信息的格式或范例。如果您正在创建一个需要管理人员信息的应用程序，您可能会选择存储他们的名字、中间名和姓氏。

当一个软件工程师在面向对象编程中创建他们自己的数据结构来管理适合他们应用程序需求的信息时，它被称为类。

## 什么是课？

让我们回顾一下上一节中关于存储人的信息的例子。下面是 Person 类在 TypeScript 中的样子(从现在开始，我将在我的所有示例中使用 TypeScript):

```
class Person {
    firstName: string;
    middleName: string;
    lastName: string;
}
```

这个类是用 Person 这个名字声明的。类的*成员*在括号内定义。在这里，我们有`firstName,` `middleName`，`lastName`，作为`string`。

`string`本身就是一个数据结构。A `string`是类似“Hello World”的文字文本这些就是人们所说的**原始数据类型**，或者内置于编程语言中的简单数据结构，不需要软件工程师来定义。

## **什么是会员？**

有两种类型的类成员需要重点关注——字段和函数(您也可以看到它们被称为方法)。

**字段**是类别的参数。在前两节的 Person 类中，字段是`firstName`、`middleName`和`lastName`。您可以添加一个类似 age 的字段，这将是另一种原始数据类型`number`。一个字段也可以是另一个类。

**函数**接受输入，对它们执行操作，有时也对其他常量执行操作，然后产生一个输出。函数通常对存储在类中的信息执行操作。

如果您经常需要确定一个人的全名，您可以创建一个名为`getFullName`的函数，将`firstName`和`lastName`合并成一个`string`。这看起来是这样的:

```
class Person {
    firstName: string;
    middleName: string;
    lastName: string; getFullName(): string {
        return this.firstName + " " + this.lastName;
    }
}
```

括号是定义输入的地方，在这里没有输入。`: string`部分意味着该函数将返回以`string`形式出现的信息。`return`是一个关键字，用于结束一个函数并“返回”输出，语句如下。

也许您想扩展这个函数，以便我们可以选择中间名是否包含在输入中。对于这个输入，我们将使用一个`boolean`，另一个表示真或假的原始数据类型。

```
getFullName(includeMiddleName: boolean): string {
    let fullName = this.firstName; if (includeMiddleName === true) {
        fullName += " " + this.middleName + " " + this.lastName;
    } if (includeMiddleName === false) {
        fullName += " " + this.lastName;
    } return fullName;
}
```

那里有很多东西要打开。我建议花一些时间来看一看，理解语法和它在做什么。本文中还有一些我没有涉及的内容:

1.  `if`如果括号中的条件为真，语句执行括号中的代码。
2.  将变量左边的内容添加到变量中。

## 最后，什么是对象？

一个对象是一个*实例*，或者一个类的单个实例。

我们已经定义了数据结构 Person 及其成员。现在，我们可以创建*一个*人并对其执行操作。我们可以为一个虚构的人物约翰·迈克尔·史密斯创建一个人物对象:

```
let person: Person = new Person("John", "Michael", "Smith");// firstName = "John"
// middleName = "Michael"
// lastName = "Smith"
```

注意:为了用这种精确的语法完成这一点，我们需要创建一个 [*构造函数*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) ，一个类的特殊方法，用于创建和初始化该类的对象实例。看起来是这样的:

```
class Person {
    firstName: string;
    middleName: string;
    lastName: string; constructor(fn: string, mn: string, ln: string) {
        this.firstName = fn;
        this.middleName = mn;
        this.lastName = ln;
    }
}
```

让我们在 person 对象上运行函数`getFullName`:

```
person.getFullName(true);// "John Michael Smith"person.getFullName(false);// "John Smith"
```

如果我们创建另一个 Person 对象并将其命名为`personTwo`，它们可以同时存在，我们可以对它们运行`getFullName`以获得不同的结果。

```
let personTwo: Person = new Person("Andrew", "James", "Thomas");person.getFullName(true);
// "John Michael Smith"personTwo.getFullName(true);
// "Andrew James Thomas"
```

## 结论

*   数据结构是用于存储信息的定义格式。
*   类是由开发人员创建的数据结构，是数据的蓝图。
*   对象是一个类的实例。

感谢您的阅读！