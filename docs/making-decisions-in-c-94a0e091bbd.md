# 用 C++做决策

> 原文：<https://medium.com/geekculture/making-decisions-in-c-94a0e091bbd?source=collection_archive---------58----------------------->

![](img/3cddafff247c06bd6a209dd39fc29db4.png)

> 在 C++中做决策需要开发人员指定一个或多个由程序评估的条件。

# 介绍

这是编程的核心话题之一，也是你会经常用到的东西。我能想到的每一种语言都有做决定的方法。这些是条件语句，支持分支行为。

做决策特别意味着你将比较数据或 C++语句。我们可以用数字或字符操作符来做到这一点。逻辑运算符从左到右工作。现在我们可以征求意见并做出决定。

# [If]语句

使用[if]可以让你在语句中做出决定。只有我们满足条件，他们才会执行。这就是[if]的工作方式。比如，如果一个条件为真，你的代码就可以跳转到某个部分，执行某个东西。如果条件为假，它将向另一个方向发展。

“if”语句的语法如下所示:

```
if (condition)
{
Do something based on condition
}
```

“if”语句的目的是评估一个条件。你可以测试某事是真还是假。一个例子是问温度是超过 100 还是低于 0。然后，在大括号之间的代码块中，程序根据条件做一些事情。

# 条件句的不利之处

我们将你写的所有条件句存储在内存中。这意味着我们的程序将不得不访问内存的不同部分。这可能会降低程序的速度。我建议不要使用大量的条件句。少量使用他们没问题，看你表现就好。

# 用 If 语句做决策

```
#include <iostream>

int main()
{
// variables
float gpa;

// get input
std::cout << "Please enter your gpa" << std::endl;
std::cin >> gpa;

// conditional
if (gpa>=3.0)
{
  std::cout << "You have good grades, nice going!" << std::endl;
}

  return 0;
}
```

这是我们的第一个例子。记住，以“//”开头的行是注释行。他们只是记录代码。

所以，在这个节目里，我想问问某人的 gpa。然后我想祝贺他们，如果它是好的。

*   创建的变量
*   从用户处获得输入
*   做了一个条件
*   输出结果

看看“if”语句是如何使用的。给它一个条件，然后做一些事情。

从代码中可以看出，我的条件是“> =3.0”。当我运行这段代码时，我输入了 3.2。我对自己寄予厚望，你也看到了！无论如何，因为条件大于或等于 3.0，它输出祝贺消息。

如果我的 gpa 低于 3.0 会怎么样？我的代码会做什么？因为如果条件为假，程序没有路径可走，所以它会做一些无用的事情。我们需要加上一个条件，占低于 3.0 的 gpa。然后，我们需要告诉程序在这一点上它应该做什么。

```
#include <iostream>

int main()
{
// variables
float gpa;

// get input
std::cout << "Please enter your gpa" << std::endl;
std::cin >> gpa;

// if conditional is true
if (gpa>=3.0)
{
  std::cout << "You have good grades, nice going!" << std::endl;
}
// if conditional is false
else
    std::cout << "You need to work harder" << std::endl;

  return 0;
}
```

在这个例子中，我添加了一个“else”条件。这告诉程序如果原始条件为假时该做什么。所以，如果我们的 gpa 低于 3.0，那么程序告诉我们要更加努力。我还对我所做的更改进行了颜色编码，这样就很容易看到了。

使用“if”语句，您可以在程序中的任何地方使用它们。可以有多个条件，做多个事情。我们也可以测试字符串或其他变量的条件。这是另一个例子。

```
#include <iostream>

int main()
{
// variables
std::string fav_pokemon;

// get input
std::cout << " Try to guess my favorite Pokemon " << std::endl;
std::cin >> fav_pokemon;

// conditional
if (fav_pokemon == "darkrai")
  std::cout << " You guessed right! " << std::endl;
else
  std::cout << " Sorry, that is incorrect " << std::endl;

return 0;
}
```

在这个节目中，我要求你们猜一猜我最喜欢的口袋妖怪。很简单，对吧！如您所见，这次我使用字符串来获取字符串输入。这非常简单，但是它向您展示了如何使用“if”语句。

还要知道我们可以测试多种情况。如果我想让猜测我最喜欢的口袋妖怪变得简单一点，我们可以把它加入到测试中。

```
#include <iostream>int main()
{
// variables
std::string fav_pokemon;// get input
std::cout << " Try to guess my favorite Pokemon " << std::endl;
std::cin >> fav_pokemon;// conditional
if (fav_pokemon == "darkrai" || fav_pokemon == "vileplume") 
 std::cout << " You guessed right! " << std::endl;
else
 std::cout << " Sorry, that is incorrect " << std::endl;return 0;
}
```

正如你在上面看到的，我测试了达克莱伊或霸王花是否正确。所以，如果一个人猜对了任何一个，那么他们就明白了我真正的意思。

# 复杂分支

之前，我们只是使用了一种简单的流量控制形式。一个“if”和一个“else”语句。然而，我们有更多的选择。可以有多个分支。我们称之为嵌套语句。

缩进每一级嵌套。这使得你的代码更容易阅读，并防止愚蠢的错误。尽量保持所有关卡都一样。您可以使用“tab”键或任何您想要的键。

小心设计你的“if else”语句。很容易得到你的程序将会编译的地方，但是你得到了错误的输出。确保使用大括号来控制程序的流程。你不希望编译器通过一个逻辑错误给出一个错误的值。这些可能很难追踪，因为没有警告。

编译器将“else”与尚未配对的最近的“if”配对。这就是大括号的作用，所以你可以确保程序按照预期的方向运行。

```
#include <iostream>

int main()
{
// variables
float gpa;

// get input
std::cout << "Please enter your gpa" << std::endl;
std::cin >> gpa;

// conditional
if (gpa >= 3.8)
  std::cout << " Those are exceptional grades " << std::endl;
else if (gpa >= 3.0)
  std::cout << "You have good grades, nice going!" << std::endl;
else
  std::cout << "You need to work harder" << std::endl;

return 0;
}
```

在新的部分中，我通过使用“else if”语句添加了另一个条件。这种技术可以让初学者精确地控制他们程序的流程。它正在测试不同的阳性条件。

现在，如果 gpa 在 3.0 到 3.8 之间，它会触发一个更准确的不同信息。如果没有一个条件成立，那么我们最后的 back 语句将会显示。

在下一个例子中，我用我的口袋妖怪猜测程序做同样的事情。

```
#include <iostream>

int main()
{
// variables
std::string fav_pokemon;

// get input
std::cout << " Try to guess my favorite Pokemon " << std::endl;
std::cin >> fav_pokemon;

// conditional
if (fav_pokemon == "darkrai")
  std::cout << " You guessed right! " << std::endl;
else if (fav_pokemon == "vileplume")
  std::cout << " I like that pokemon too ";
else
  std::cout << " Sorry, that is incorrect " << std::endl;

return 0;
}
```

我没有在我的条件语句中使用“或”或“和”逻辑，而是将其分解，使用“否则如果”。这只是做这件事的另一种方法。记住，我们从顶部检查条件。一旦条件为真，就跳过块的其余部分。

请记住,“if”块之外的任何语句都将始终运行。这是因为它不是条件的一部分，所以不会被检查，它是一个常规的 C++语句。

# 结论

C++是一门非常酷的语言。尽管年代久远，但它仍在不断更新，是目前最热门的语言之一。对于硬件编程来说，它仍然是王道，而且不会很快取代它的位置。不过，除了硬件编程，你还可以做更多的事情。

# 感谢阅读

谢谢你读了这篇文章，我真的很感激。

如果你想加入我的简讯，你可以在这里:

[*杰森的迅*](https://mailchi.mp/70ab982b2619/computing)

如果你需要关于下一步该读什么的建议，那么试试这些 C++文章:

[*初学 C++的*](https://jason-46957.medium.com/learning-c-for-beginners-62ad50876d64)

[*在 C++中使用变量*](https://jason-46957.medium.com/using-variables-in-c-621c9e150650)

*原载于 2021 年 8 月 2 日 https://aindien.com**的* [*。*](https://aindien.com/making-decisions-in-c.html)