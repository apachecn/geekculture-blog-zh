# 揭开 C++左值和右值之谜(一)

> 原文：<https://medium.com/geekculture/unveiling-c-lvalues-and-rvalues-mysteries-i-a829ec5a4aad?source=collection_archive---------25----------------------->

![](img/5ebb25926376275364c3a763bcbab424.png)

Photo by [Mohammad Rahmani](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code-rvalue?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 第一部分:左值和右值基础。

自从 C++11 出现以来，左值和右值的含义和重要性已经扩展了。它们是需要理解的重要概念，因为它们有很多用途，你会发现它们与很多概念都有联系，比如左值/右值引用、移动语义、完全转发和移动构造函数等等。在这个由两部分组成的系列中，我们将展开所有这些元素，为您即将到来的 C++项目提供可理解和可操作的知识。

在 C++的过去版本中，**左值**和**右值**的概念并没有那么重要。这仅仅是在陈述中指出价值立场的一种区别。

```
int value = 7;
```

在下面的示例中，**左值**是名为“*值”*的整数类型，而**右值**是“7”。所以基本上，**左值**位于语句的左边，**右值**位于右边。但是，这些概念被延伸了，在今天，它们的意义和作用更加重要。

左值是指向内存位置的任何东西，因此你可以得到它的地址。因此，**左值**作为存储在内存中的变量，寿命更长。相比之下，**右值**是你得不到它的地址，不指向任何地方，并且寿命很短的东西。

把它们放在一起考虑，因为它们就像一个泡菜坛子:左值就是坛子本身，充当一个容器。**右值**是罐子里的东西(这里是泡菜)。

![](img/338d3a1df8077b790c1aec58af8f1370.png)

Photo by [SuckerPunch Gourmet](https://unsplash.com/@suckerpunchgourmet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/pickle-jar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

所以，回到我们的例子，让我们扩展一下:

```
int value = 7;
```

在这种情况下，*值*是一个**左值**，因为它是一个将要存储在内存中的变量，而数字“7”是一个**右值**，因为它是一个*文字常量*，除了程序执行期间的一些临时寄存器之外，它没有显式的内存地址。所以'*值'*是我们的罐子' *7* '是我们的泡菜。很简单！

一个语句中可以有两个左值。让我们关注这段代码的第二行:

```
int value = 7;
int * pointerToValue = &value;
```

我们已经确定'*值'*是一个**左值**。更进一步， *pointerToValue* 是一个**左值**，因为它是一个指针变量，通过使用操作符 *' & '* 指向*值*的地址。

然而，如果我们尝试这样做，会发生什么呢？

```
int value;
7 = value;
```

您将得到一个编译器错误，因为表达式是不可赋值的。 *7* 是一个**右值**，所以它没有被分配一个特定的内存位置。您试图将'*值'*赋给数字中间状态，这就是为什么编译器需要一个**左值**作为左操作数。但是重要的是要注意，这不仅仅是一个关于**右值**左侧位置的问题。当您写时，使用右值**作为右操作数会出现类似的问题:**

```
int * pointerToValue = &7;
```

在这种情况下，编译器会报错，因为一元运算符 *' & '* 无法获取 int 类型的**右值**的地址，需要一个**左值**。如果您试图获取一个**左值**和一个**右值**之间的操作的地址，也会发生类似的事情:

```
int value;
// This line will not compile
int * pointerToValue = &(7 + value);
```

一个有趣的发现是，您甚至可以将前缀递增运算符与 address-of 运算符结合使用，如下所示:

```
int value = 7;
int * pointerToValue = &++value;
// Prints 8
std::cout << * pointerToValue << std::endl;
```

但是，您不能将这种组合与后增量运算符一起使用。这背后的原因是 postfix ++有一个伪参数来存储变量的当前值，该值将在之后立即递增。这个临时参数是后递增运算符返回的一个右值，因此编译器无法得到它的地址:

```
int value = 7;
// This line will not compile
int * pointerToValue = &value++;
```

**功能和对象。**

现在是时候检查**左值**和**右值**如何与函数一起工作了。考虑以下代码:

```
#include  <cstdlib>int globalValue = rand()%10;int someValue()
{
 return rand()%100;
}int& returnGlobalValue()
{
 return globalValue;
}int main ()
{
 someValue() = rand()%1000; returnGlobalValue() = someValue();
 returnGlobalValue() = rand()%10000;

 return 0;
}
```

首先要考虑的是 *rand()* 函数调用会返回一个**右值**。所以当我们试图给函数 *someValue()，*赋值的时候，编译器会大喊需要一个**左值**作为赋值的左操作数:

```
//This line will not compile
 someValue() = rand()%1000;
```

此外，在 *somevalue()* 函数中不能有返回引用。您将得到一个无效的初始化，因为对整数类型的**左值**引用不能与由 *rand()* 函数生成的**右值**绑定:

```
//This line will not compile
int& someValue()
{
 return rand()%10;
}
```

然而，我们有一个函数返回对左值的引用。 *returnGlobalValue()* 指向全局变量 *globalValue* 的内存位置。因为它是可赋值的，所以下面几行代码是有效的:

```
returnGlobalValue() = someValue()
returnGlobalValue() = rand()%10000;
```

现在来说说对象。我们将定义一个非常简单的类和一些对象，然后我们将逐步剖析它们:

```
class AnObject
{
//…
 public: AnObject(){}
  AnObject(int param){}};AnObject getAnObject()
{
 return AnObject();
}int main ()
{
 AnObject Object = getAnObject();
 AnObject *pointerToObject = &Object1;
 AnObject *pointerToObject2 = &getAnObject();
}
```

你现在可以假设*对象*是一个**左值，**并且它被初始化没有问题，因为 *getAnObject()* 函数返回一个**右值**然后分配给它。与我们的变量示例相似， *pointerToObject* 是一个**左值，**因为它是一个指针对象，指向“*对象”*的地址。

```
//This works fine
 AnObject Object = getAnObject();
 AnObject *pointerToObject = &Object;
```

此外，您也许能够理解定义*pointer tobject 2*的代码行不会编译，因为我们试图获取一个临时对象的地址。

```
//This line will not compile
 AnObject *pointerToObject2 = &getAnObject();
```

干得好！现在你可以理解基本概念了。没那么难，对吧？在下一篇文章中，我们将进一步探索并讨论**左值**和**右值**之间的引用和转换。直到下一次，编码快乐！