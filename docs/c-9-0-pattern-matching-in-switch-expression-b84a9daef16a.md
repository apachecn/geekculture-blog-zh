# C#9.0 开关表达式中的模式匹配

> 原文：<https://medium.com/geekculture/c-9-0-pattern-matching-in-switch-expression-b84a9daef16a?source=collection_archive---------3----------------------->

## 开始使用模式组合子构建高效的算法

![](img/020f7f4420da6d89e2cf23f7da26c679.png)

Photo by [Maxwell Nelson](https://unsplash.com/@maxcodes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/c%23?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

C# 9 带来了许多激动人心的[特性和语言增强](https://docs.microsoft.com/en-gb/dotnet/csharp/whats-new/csharp-9)。在本文中，我们将详细探讨使用开关表达式和模式组合符的新模式匹配。

## 先决条件

首先，请安装[。NET 5](https://dot.net/get-dotnet5) (如果你还没有的话)，因为它包含了 C# 9 编译器。

## 模式匹配—它是什么？

这是一种帮助你识别表达式是否具有某些特征的技术。它非常适合处理不相关系统中的对象以及处理来自多个数据源的数据的情况。

模式匹配最初是在 C# 7 中使用“is 表达式”和“switch 语句”引入的。在新版本中，这些功能得到了扩展。

## **申报模式:**

考虑以下代码:

```
int? result= 100; if(result is int number)
{Console.WriteLine( $”The Result has value {number}”);
}else{ Console.WriteLine(“Result is null”); }
```

它使用**声明模式，(**简单地检查一个表达式的类型是否与给定的类型相匹配)并测试变量结果是否为整数类型，如果为真，则将它赋给变量‘number’。

## **逻辑模式:**

在 C#9.0 中，通过将“is”运算符与“not”模式结合使用，可以使用它来检查空引用值:

```
string? result = "This is not null string";if(result is not null)
{
   Console.WriteLine(result);
}
```

和'，'或'是 C#9.0 中新的模式组合子，可用于创建逻辑模式。

## **C # 9.0 中的关系模式和模式组合子**

关系模式允许您将表达式结果与常量(可以是 int、float、char 或 enum 类型)进行比较。它使用类似于、< = or > =的关系操作符。它可以与“and”模式结合使用，以检查某个值是否在某个范围内。

在下面的示例中，我们创建了检查血氧饱和度值的方法，并确定是否需要任何医疗干预。

```
static string FindOxygenSaturation(int saturation) =>saturation switch{>= 95 => "Normal, no significant intervention needed",91 or 92 or 93 => "Keep monitoring",>= 85 and <= 93 => "Initiate oxygen therapy",<85 and >0 => "Severly Hypoxic! Administer supplemental oxygen immediately",_ => throw new ArgumentOutOfRangeException(nameof(saturation),$"Invalid value:{saturation}"),};
```

您可以看到如何使用“and”模式来检查血氧饱和度值是否在 85 到 93 的范围内。

析取 or 模式可用于我们希望将输入与任一模式进行比较的情况。在上面的例子中，如果氧饱和度等于 91、92 或 93，它将返回“保持监测”。

## **开关表达式:**

您还可以注意到，上面的示例使用了“switch expression ”,而不是带有 case 块的经典 switch 语句。(它是在 C#8.0 中引入的，使开发人员能够使用更简洁的语法，并避免重复的关键字，如 case 和 break，使其更具可读性)。

开关表达式的基本元素如下:

*   一个表达式，后跟 switch 关键字，在上面的例子中是“saturation”方法参数。
*   开关表达式臂由逗号分隔，并包含一个模式、一个= >标记和一个表达式。

它还使用一种使用下划线(_)的丢弃模式来处理与任何表达式都不匹配的任何整数值。

## **护箱**

switch 表达式可能包含一个可选的“大小写保护”,可以在模式本身不够用的情况下使用。

例如，可以在 OxygenSaturation 示例中添加以下开关表达式，以便仅在值不超过 int.MinValue 时检查零。

```
<0 when saturation > int.MinValue => "Error;",
```

## **房产模式:**

它是在 C# 8.0 中引入的，您可以使用属性模式将表达式的属性与任何嵌套模式进行匹配。

例如，以下示例使用属性模式和嵌套模式来标识使用给定日期的会议日期。

```
static bool IsMeetingDay(DateTime date) => 
date is { Year: 2021, Month: 5 or 6, Day: 07 or 20 or 30}
```

## **元组模式:**

一些功能可能有多个输入。这就是元组模式的用武之地——它允许对多个值使用 switch 表达式作为“元组”。

著名的游戏“石头”、“布”、“剪刀”就是使用元组模式的一个很好的例子。

```
public static string RockPaperScissors(string first, string second)=> (first, second) switch{("rock", "paper") => "rock is covered by paper. Paper wins.",("rock", "scissors") => "rock breaks scissors. Rock wins.",("paper", "rock") => "paper covers rock. Paper wins.",("paper", "scissors") => "paper is cut by scissors. Scissors wins.",("scissors", "rock") => "scissors is broken by rock. Rock wins.",("scissors", "paper") => "scissors cuts paper. Scissors wins.",(_, _) => "tie"};
```

C# 9.0 还增加了对带括号模式的支持，在这种模式下，你可以简单地在任何模式周围加上括号来强调或改变优先级。

```
char c = 't';if ((c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z')))Console.WriteLine("Char is alphabet");
```

您可以看到当与模式组合一起使用时，这种模式是如何允许实施优先级的。

## 摘要

在本文中，我们探讨了 C# 9.0 是如何引入模式和模式组合子的，这些模式和模式组合子可以在 switch 表达式中用来构建类型驱动和数据驱动的算法，以便以高效的方式处理数据。