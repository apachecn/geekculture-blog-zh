# 编程中的决策

> 原文：<https://medium.com/geekculture/decision-making-in-programming-743e86895ce4?source=collection_archive---------18----------------------->

![](img/fe1b2ddedbc22c297be9588cf74142c8.png)

我们 [已经看到了从用户那里获取输入的方法。](/geekculture/how-to-interact-with-users-in-programming-d9eaebb565c1)也许有一天，我们需要指导计算机根据输入做出决定。例如，在一个测验程序中，我们问用户答案，然后我们需要检查用户是否输入了正确的答案。这个指导计算机做出决定的过程在编程中被称为“决策”。

# 如果和否则

if 和 else 语句检查条件是真还是假。如果条件为真，它执行某个特定的代码集，如果条件为假，它执行另一个代码集。

```
if (num % 2 == 0)
{
    printf("Number is even");
}
else
{
    printf("Number is odd");
}
```

在“if”后面的括号中，我们提供的条件是(num % 2 == 0)。“if”语句检查“num”除以 2 是否得到余数 0。如果条件为真，那么它将执行花括号之间的代码，即显示“数字是偶数”的代码。如果条件变为 false，那么它将执行 else 后面的花括号之间的代码，这是显示“Number is odd”的代码。

# if，else 和 else if

如果我们需要检查更多的条件呢？我们可以使用额外的“else if”语句来链接“if 和 else”语句。

```
if (num > 0) 
{
    printf("Number is positive");
}
else if (num < 0)
{
    printf("Number is negative");
}
else
{
    printf("Number is 0");
}
```

这里,“if”语句检查“num”是否大于零。如果为真，则显示“数字为正”。之后，它检查“num”是否小于 0。如果为真，则显示“数字为负”。如果两个条件都为假，则执行“else”语句，并显示“Number is 0”。我们可以在一个“if，else 和 else if”块中包含任意多的“else if”语句。

如果条件中只有一行代码，就没有必要加上花括号。它是可选的。下面的代码也可以工作，因为在“if，else 和 else if”语句之后只有一行代码。

```
if (num > 0) 
    printf("Number is positive");
else if (num < 0)
    printf("Number is negative");
else
    printf("Number is 0");
```

# 等级计算程序

```
#include <stdio.h>int main()
{
    float marks;
    char grade;

    printf("Enter marks: ");
    scanf("%f", &marks);

    if(marks >= 90)
    {
        grade = 'A';
    }
    else if(marks >= 80 && marks < 90)
    {
        grade = 'B';
    }
    else if(marks >= 70 && marks < 80)
    {
        grade = 'C';
    }
    else if(marks >= 60 && marks < 70)
    {
        grade = 'D';
    }
    else if(marks >= 50 && marks < 60)
    {
        grade = 'E';
    }
    else 
    {
        grade = 'F';
    }

    printf("Your grade is %c", grade);

    return 0;
}
```

这里，在用户输入标记后，程序检查“if 和 else if”语句中的每个条件是否为真。如果条件为真，则执行该特定代码块。如果没有一个条件为真，则执行“else”语句中的代码块。通过这些程序，程序计算输入分数的等级。

# 三元运算符

三元运算符可用作“if 和 else”语句的快捷方式。它需要三个操作数。条件后面跟一个问号，然后是条件为真时要执行的表达式，后面跟一个冒号(:)，最后是条件为假时要执行的表达式。

```
printf("%s", num % 2 == 0 ? "Number is even" : "Number is odd");
```

这里，在 printf()内部，在格式说明符" %s" 之后，我们有一个三元运算符，而不仅仅是一个变量。在运算符中，有一个条件检查“num”除以 2 是否得到余数 0。如果条件为真，则取问号后的表达式，即“数为偶数”否则取冒号后的表达式，即“数为奇数”。

使用三元运算符，可以用更少的代码重新编写分数计算程序。

```
#include <stdio.h>
int main()
{
    float marks;
    char grade;

    printf("Enter marks: ");
    scanf("%f", &marks);

    grade = marks >= 90 ? 'A' : 
            marks >= 80 && marks < 90 ? 'B' :
            marks >= 70 && marks < 80 ? 'C' :
            marks >= 60 && marks < 70 ? 'D' :
            marks >= 50 && marks < 50 ? 'E' : 'F';

    printf("Your grade is %c", grade); return 0;}
```

最初，条件是“分数≥ 90”。如果条件为真，则在问号后执行表达式，因此将“A”赋给“grade”。如果条件为假，则执行冒号后的表达式，并检查下一个条件是否为真，依此类推。但是，如果条件为真或假，则不能执行多行代码，因此只能使用三元运算符来代替简单的“if 和 else”语句，每个块中只有一行代码。

[还记得吗，我们以前是怎么打印布尔值的？](/geekculture/the-logic-in-logical-operators-1cb16f4f0af3)三元运算符可用于打印布尔值。

```
#include <stdio.h>
#include <stdbool.h>
int main()
{
    int num1, num2;
    bool isEqual;
    printf("Enter a number: ");
    scanf("%d", &num1);
    printf("Enter a number again: ");
    scanf("%d", &num2);

    isEqual = num1 == num2;
    printf("%s", isEqual ? "true" : "false");

    return 0;
}
```

三元运算符检查“isEqual”的值是 true 还是 false。如果为真，则打印“真”，否则打印“假”。

[**Previous = >编程中如何与用户互动？**](/geekculture/how-to-interact-with-users-in-programming-d9eaebb565c1)