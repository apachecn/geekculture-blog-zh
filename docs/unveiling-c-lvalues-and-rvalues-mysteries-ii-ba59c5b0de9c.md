# 揭开 C++左值和右值的神秘面纱(二)

> 原文：<https://medium.com/geekculture/unveiling-c-lvalues-and-rvalues-mysteries-ii-ba59c5b0de9c?source=collection_archive---------3----------------------->

![](img/dc9168f6eae11307111dd7b5ce2c47fe.png)

Photo by [Mohammad Rahmani](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code-rvalue?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 第二部分:左值和右值引用。

自从 C++11 出现以来，左值和右值的含义和重要性已经扩展了。它们是需要理解的重要概念，因为它们有很多用途，你会发现它们与很多概念都有联系，比如左值/右值引用、移动语义、完全转发和移动构造函数等等。在这个由两部分组成的系列中，我们将展开所有这些元素，为您即将到来的 C++项目提供可理解和可操作的知识。

在过去的文章中，我们探索了关于**左值**和**右值**的基础知识，并且探索了它们如何与函数和对象一起工作。现在我们有了理解左值引用和右值引用的基础，这是 C++11 中引入的一对概念。

首先，我们将观察使用**左值**引用会发生什么。考虑以下代码:

```
class AnObject
{
//…
 public:AnObject(){}
  AnObject(int param){}};AnObject getAnObject()
{
 return AnObject();
}int main ()
{
// Lvalue refs AnObject Object;
 AnObject &refToObject = Object;
 AnObject &refToObject2 = getAnObject();
 const AnObject &refToObject3 = getAnObject();
 AnObject Object2(AnObject());
}
```

对对象的引用工作得很好，因为'*对象*是一个**左值，**并且您正在使用'*reftooobject ':*创建该对象的替代名称

```
//This works fine
 AnObject &refToObject = Object;
```

正如您可能预料的那样，通过给别名分配一个临时的**右值** a 来创建别名(一个对象的**左值**引用)根本不会起作用:

```
//This line will not compile
 AnObject &refToObject2 = getAnObject();
```

但是有一个解决方法。记住:当你创建一个引用时，你给了同一个对象另一个名字，所以对一个对象名字的任何改变总是影响另一个。因此，通过使我们的引用保持不变，**右值**的生命周期得到了延长，因为我们正在创建的别名不能被修改。

```
//This line will compile
 const AnObject &refToObject3 = getAnObject();
```

出于好奇，甚至可以通过在内部对象中使用与**右值**相同的类来进行某种嵌套的对象初始化，如下所示:

```
//This line may or may not compile depending on the compiler you use
 AnObject Object2(AnObject());
```

然而，根据您使用的编译器，您将得到混合的结果，其中一些不允许您编译这一行。由于内括号的原因，一些编译器可能会感到困惑，所以您可能会结束声明一个名为 *Object2* 的函数，而不是创建一个对象。即使代码编译成功，并且圆括号作为函数声明消除了歧义，程序也可能产生意外的结果，甚至崩溃。

一种常见的解决方案是使用额外的括号，因此不可能将表达式解析为函数声明，如下所示:

```
//This line will compile
 AnObject Object2((AnObject()));
```

另一种解决方案是使用参数化的构造函数，而不是默认的构造函数，这样就不会有歧义了:

```
//This line will compile with a parameterized constructor
 AnObject Object2(AnObject(1));
```

但是从 C++11 开始，统一初始化提供了一个非常好的解决方案:

```
//This line will compile using uniform initialization
 AnObject Object2(AnObject{});
```

所以现在我们讨论了左值引用的基础知识。让我们看看什么是**右值**引用。首先，我们可以通过使用一个**右值**引用使下面的代码有效:

```
// As you remember, this will not compile
AnObject &refToObject2 = getAnObject();
```

我们只需要将引用改为**右值**引用操作符'& &'，现在我们的代码是有效的:

```
// And now this will compile
AnObject &&refToObject2 = getAnObject();
```

当然，如果我们用对象构造函数绑定，我们现在也可以编译这一行。

```
// This also will compile
AnObject &&refToObject2 = AnObject();
```

右值引用对于函数重载也很有用。让我们定义一个函数的两个版本:

```
//Lvalue test
void test(AnObject &testObject)
{
   std::cout << "testObject is a lvalue" << std::endl;
}//Rvalue test
void test(AnObject &&testObject)
{ 
  std::cout << "testObject is a rvalue" << std::endl;
}
```

将根据用作参数的对象类型调用每个版本:

```
AnObject testObject;test(testObject);
//Output: testObject is a lvaluetest(getAnObject());
//Output: testObject is a rvaluetest(AnObject());
//Output: testObject is a rvalue
```

现在让我们尝试一些不同的方法，用一个整数作为参数创建两个函数:

```
void testInteger(int &i)
{
 std::cout << “lvalue reference “<< i << std::endl;
}void testInteger(int &&i)
{
 std::cout << “rvalue reference “<< i << std::endl;
}int main ()
{
 int x = 10;
 testInteger(x);
 testInteger(5);
 return 0;
}
```

正如您所料，第一个调用将触发**左值**版本:

```
int x = 10;
testInteger(x);
//Output: lvalue reference 10
```

此外，第二个调用通过触发**右值**版本表现出预期的行为:

```
testInteger(5);
//Output: rvalue reference 5
```

但是，如果我们添加第三个版本的函数 *testInteger()* 并将整数作为参数，会发生什么情况呢？我们将得到一个错误，因为重载函数的调用不明确，编译器将无法区分该函数和*test integer(int&I)*(lvalue 版本):

```
//This will not compile
void testInteger(int i)
{
   std::cout << "No reference "<< i << std::endl;
}
```

另一个观察是前缀和后缀增量操作符如何与我们的重载函数交互。如果我们用后缀操作符调用函数，将调用**右值**版本，因为在 x 的增量之前将创建一个临时值:

```
testInteger(x++);
//Output: rvalue reference 10std::cout << “x value “<< x << std::endl;
//Output: x value 11
```

另一方面，如果我们调用带有前缀运算符的函数，将会调用**左值**版本，因为 x 在作为参数传递之前会递增:

```
testInteger(++x);
//Output: lvalue reference 12std::cout << "x value "<< x << std::endl;
//Output: x value 12
```

上面所有的例子都很容易理解，你不觉得吗？然而，掌握**左值**和**右值**引用之间的区别是至关重要的。原来，**右值**是用来在我们创建对象时提高代码效率的(使用一种叫做“移动构造函数”的东西)，但那是另一个故事了。在那之前，编码快乐！