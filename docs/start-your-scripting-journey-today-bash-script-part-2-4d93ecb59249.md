# 立即开始您的脚本之旅| Bash 脚本—第 2 部分

> 原文：<https://medium.com/geekculture/start-your-scripting-journey-today-bash-script-part-2-4d93ecb59249?source=collection_archive---------8----------------------->

## 编写 Bash 脚本需要知道的一切

![](img/20f49850b3cd91004a3a85428223f9fc.png)

Photo by [jules a.](https://unsplash.com/@julesea?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 前一部分:

[**立即开始您的脚本之旅| Bash 脚本—第 1 部分**](https://levelup.gitconnected.com/start-your-scripting-journey-today-bash-script-part-1-46cbddf4e4e7)

## 决策

Linux bash/shell 脚本用于自动化，自动化通常需要决策。类似于其他编程语言，我们可以使用 **if /else** 语句进行决策运算。我们还可以在 **if/else** 语句之间使用 **else if (elif)** 语句。根据我们的需要，也可以创建嵌套的 **if/else** 语句。

**结构**

```
**if [ expression 1 ]**
then
   Statement to be executed if expression 1 is true**elif [ expression 2 ]**
then
   Statement to be executed if expression 2 is true**elif [ expression 3 ]**
then
   Statement to be executed if expression 3 is true**else**
   Statement to be executed if no expression is true
fi
```

我们需要使用各种运算符如 **(==，！=，-eq，-nq)** 按要求决策。从[这里](https://www.tutorialspoint.com/unix/unix-basic-operators.htm)了解更多关于运营商的信息。

## 任务 1

写一个脚本，其中你将接受一个数字作为用户输入，然后使用 **if else** 语句我们将检查这个数字是偶数还是奇数

## **任务二**

为评分系统编写一个 bash 脚本:

```
**marks(80-100)** = First division
   **marks(95-100)** = 100% Scholarship
  ** marks(80-94)**  = 50% Scholarship
**marks(60-79**) = Second division
**marks(40-59)** = Third division
**marks(0-39)** = Failed
```

## 用线串

当编写 Bash 脚本时，我们经常需要使用字符串执行各种操作。比如检查一个字符串是否为空，比较两个字符串，检查子字符串，等等。

**演示文稿**

**子串
要检查一个字符串是否包含一个子串，其中一种方法可以是使用星号符号`*`进行模式匹配。`*`表示零个或多个字符。比如说—**

> 你好，日安！
> *好*

有多种方法可以检查一个字符串是否包含子串。

一种方法是用星号符号`*`将子字符串括起来，这意味着匹配所有字符。

## **任务— 3**

使用 bash 脚本检查子字符串。

## 排列

Bash 数组的主要目的是以索引的方式存储信息。

**演示脚本:**

## 环

**循环——演示脚本**

使用**遍历列表进行循环:**

使用**遍历数组进行循环:**

**While 循环—**

在循环下，我们可以使用 **if/else** 语句，同样类似于其他编程语言我们也可以使用 **break** 和 **continue** 关键字来控制循环。

## 任务— 4

使用 **while** 循环和 **if/else** 语句编写脚本，并尝试使用 **break** 和 **continue** 关键字。

## 任务— 5

使用**为**循环编写一个脚本，只显示从 **1 到 99** 的奇数自然数。

## 功能

函数是重用代码的好方法。在 bash 脚本中，如果将函数视为脚本中的一个小脚本，理解函数会更容易。并且在调用函数之前，函数定义必须出现在脚本中。

**功能结构:**

```
**function_name () {
    <commands>
}**0r**function function_name {
    <commands>
}**
```

**参数:** 在 bash 脚本中，我们不能在括号()内传递参数。如果我们需要向函数发送参数，我们必须遵循类似的方式将命令行参数传递给脚本。与命令行参数类似，我们可以直接在函数名后面提供参数。在函数中，参数将被访问为 **$1，$2…$9** 。

**演示文稿:**

**返回值:**
与其他**编程语言**不同，我们不能使用 **return** 关键字将数据发送回调用位置。但是，当您运行一个函数时，它还会返回该函数中最后一个运行命令的退出状态。
在 Bash 脚本中，返回值被赋给 **$？**可变。如果最后一个运行命令执行成功，那么返回值将为 0。否则，该函数将返回一个非零值。我们还可以使用 **return** 关键字设置一个 **return** 值，但是该值必须是一个数值。

**演示脚本:**

**局部变量:** 我们可以使用**局部**关键字声明函数作用域的局部变量。

**演示脚本:**

## 实践

让我们尝试使用 bash 脚本解决 leetcode.com 的以下问题。

**问题名称:**第十行

[](https://leetcode.com/problems/tenth-line/) [## 第十行— LeetCode

### 给定一个文本文件 file.txt，只打印该文件的第 10 行。示例:假设 file.txt 包含以下内容…

leetcode.com](https://leetcode.com/problems/tenth-line/) 

**解决方案:**

**问题名称:**转置文件

[](https://leetcode.com/problems/transpose-file/) [## 转置文件— LeetCode

### 给定一个文本文件 file.txt，转置其内容。你可以假设每一行都有相同的列数…

leetcode.com](https://leetcode.com/problems/transpose-file/) 

**解决方案:**

你也可以在 [**hackerrank**](https://www.hackerrank.com/domains/shell?filters%5Bsubdomains%5D%5B%5D=bash) 上练习 shell 脚本

## 接下来—

[](/geekculture/start-your-scripting-journey-today-bash-script-part-3-9779ac0abea2) [## 立即开始您的脚本之旅| Bash 脚本—第 3 部分

### 编写 Bash 脚本需要知道的一切

medium.com](/geekculture/start-your-scripting-journey-today-bash-script-part-3-9779ac0abea2) 

> *如果你觉得这篇文章很有帮助，请* ***别忘了*** *点击* ***跟随*******拍拍*** *按钮，帮我写更多这样的文章。* ***谢谢🖤****