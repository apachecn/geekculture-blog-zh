# 为什么您应该让您的预处理器休息一下

> 原文：<https://medium.com/geekculture/how-your-preprocessor-is-hurting-your-programs-4b92eebd74e3?source=collection_archive---------28----------------------->

![](img/a50783a1c3c9032cb7ec2ac85632ae96.png)

Source: [Udacity](https://www.udacity.com/blog/2021/03/how-long-does-it-take-to-learn-c.html)

*免责声明:这些条目最初由 Scott Meyers 改编自 Effective C++。这是我回顾和总结 Meyers 讨论的 55 个项目的系列文章中的第一篇。这些帖子的目的是作为我个人对所讨论的概念的回顾，也为读者提供一个带有附加评论的精简版本。*

## 什么是编译器和预处理器？

执行一个用 C++这样的高级语言编写的程序要求程序被分解成你的 CPU 能够理解的 1 和 0。这种语言被称为机器语言。C++和其他高级语言使我们不必编写乏味的汇编语言甚至二进制语言。计算机最底层是电子开关、电缆和逻辑门。为了让你的 C++被分解成机器语言，它需要被转换。编译器把你的程序转换成你的系统可以执行的机器码文件。

当编译器第一次在你的代码上运行时，预处理器开始工作。在 C++中，预处理器首先查找以“#”开头的指令行预处理器指令实质上是在编译开始前给编译器一些指令。预处理器通常用于包含其他库和头文件(#include)，但是，还有许多其他情况下也经常使用它们，例如定义类似函数的宏、常量、条件编译等等。

## 预处理器问题

Meyers 开始用例子列出预处理器的问题

```
#define ASPECT_RATIO 1.653
```

预处理器可能会在源代码到达编译器之前删除整个语句。根据 Meyers 的说法，“如果你在编译过程中得到一个涉及到使用常量的错误，这可能会令人困惑，因为错误消息可能指的是 1.653，而不是 ASPECT_RATIO。”这个问题是由于预处理器从编译器中忽略了符号常量，所以符号常量从未被放入符号表中。最好用常量替换宏，例如

```
const double AspectRatio = 1.653;
```

在符号常量上使用语言常量，可以确保编译器看到一个表达式，并将其适当地放在符号表中。另一个好处是你的代码也变小了。使用#define 可能会导致预处理器盲目地在目标代码中替换 ASPECT_RATIO。语言常量不会产生一个以上的副本。

## 特殊情况

当用常量替换#defines 时，Meyers 列出了两种特殊情况。第一种情况涉及常量指针。除了所指向的数据之外，声明指针本身的 const 也很重要。根据 Meyers 的说法，这是因为常量定义放在头文件中，许多不同的源文件都会包含它们。下面是一个在头文件中定义基于常量 char*的字符串的例子。

```
const char * const authorName = "Masashi Kishimoto";
```

代码片段的重点是强调当声明一个指针和它指向的项 const 时，关键字必须使用两次。Meyers 指出，使用字符串对象比使用基于 char*的对象更好。

```
const std::string authorName("Masashi Kishimoto");
```

第二种特殊情况涉及特定于类的常数。使 const 成为类的成员会将其范围仅限于类。使它成为静态的还可以确保最多有一个 const 的副本。

```
class GamePlayer{
private:
    static const int NumTurns = 5;
    int scores[NumTurns];
};
```

特定于类的静态和整型常量是一个例外，所有的东西都必须在使用前定义。上面的例子是 NumTurns 的声明，而不是定义。根据 Meyers 的说法，“只要你不记下他们的地址，你就可以在不提供定义的情况下声明和使用他们。”如果您要记录地址，则需要一个单独的定义，例如:

```
const int GamePlayer::NumTurns;
```

如果没有定义值，这怎么可能是定义呢？因为这个代码片段将放在实现文件中，而不是头文件中。类常数的初始值是在声明该常数时提供的(例如，NumTurns 在声明时被初始化为 5)

需要注意的是#define 也不考虑作用域，所以实际上没有特定于类的方法来使用#define 指令创建常量。"一旦定义了一个宏，它将在编译的其余部分生效."缺乏对范围的考虑也意味着没有宏观的方法来提供封装。

Meyers 继续指出“类内初始化只允许用于整型和常量。在不能使用上述语法的情况下，您可以在定义时使用初始值:"

```
class CostEstimate{
    private:
        static const double FudgeFactor; //declaration goes in //header
};const double CostEstimate::FudgeFactor = 1.35; //definition goes in //implementation
```

## 枚举黑客

大多数时候，上面的例子就足够了。但是，当您在编译类的过程中需要类常量的值时，可能会出现异常，例如，带有为 size 传递的常量的数组声明。enum hack 是一种补偿编译器的方法，编译器禁止对静态整型类常量的初始值进行类内指定。这是一个聪明的方法，它利用了枚举类型值可以在需要 int 的地方使用的事实。

```
class GamePlayer{
    private:
        enum {NumTurns = 5}; //NumTurns is symbolic name for 5
        int scores[NumTurns];
};
```

Meyers 指出“enum hack 的行为在某些方面更像#define，而不是 const，有时这就是你想要的。”我们可以接受一个常量的地址，但是，这对于 enums 和#define 来说都是非法的操作。使用 enum hack 可以防止有人试图获取你的常量积分的指针或引用。枚举常量还可以防止不必要的内存编译，这些编译可能是由糟糕的编译器执行的。Meyers 在结束对 enum hack 的讨论时指出，enum hack 是模板元编程的一项基本技术，当您看到它时，如果能够识别它就好了。

## 回到预处理器

实现看起来像函数的宏是#define 的另一个常见误用。

```
//call f with the maximum of a and b
#define CALL_WITH_MAX(a,b) f((a) > (b) ? (a) : (b))
```

缺点包括必须将宏体中的所有参数括起来，以避免意外行为。迈耶斯指出，即使遵循这一范式，“奇怪的事情也可能发生。”

```
int a = 5, b = 0;
CALL_WITH_MAX(++a,b); //a is incremented twice
CALL_WITH_MAX(++a, B=10); //a is incremented once
```

通过利用内联函数的模板，您可以获得宏的效率和函数的安全性。

```
template<typename T>
inline void callWithMax(const T& a, const T& b){
    f(a > b ? a : b);
}
```

在上面的例子中，我们通过引用 const 来传递，因为我们不知道 T 是什么。因为这个函数是一个实函数，所以作用域和访问规则是受尊重的。根本没有办法用宏实现特定于类的内联函数。

## 结论

除了通常更安全和更有效之外，利用上面的方法将会减少预处理器产生的错误，并节省调试时间。但是，有些情况下使用预处理器是不可避免的(#include、条件编译等)。迈耶斯列举了两件需要记住的事情:

*   对于简单的常量，最好使用常量对象或枚举，而不是#defines
*   对于像宏这样的函数，最好使用内联函数而不是#defines