# Python 到 C++，数据科学家学习新语言的旅程——函数

> 原文：<https://medium.com/geekculture/python-to-c-a-data-scientists-journey-to-learning-a-new-language-functions-ba9d6ce222e3?source=collection_archive---------65----------------------->

![](img/890736ea088de4cf94031ae66343a512.png)

## 介绍

勇敢的读者们，欢迎回到 Python 到 C++系列的下一期。在上一期中，我们学习了条件、条件语句，并使用我们所有的新知识用 Python 和 C++从头构建了一个简单的计算器。在这次安装中，我们将进入一个更高级的主题，学习我们感兴趣的编程语言中函数和函数创建的来龙去脉。我们将在本文中使用的原始代码可以在以下位置找到:

[Python](https://github.com/Daniel-Benson-Poe/Python_to_Cpp_Series/blob/main/Article_5/python/functions.py)

[C++](https://github.com/Daniel-Benson-Poe/Python_to_Cpp_Series/blob/main/Article_5/cpp/functions.cpp)

## 函数的功能

那些以前有过 Python(甚至 C++)经验的人可能已经熟悉了这个函数，这让您知道了它在任何主要编程项目中的重要性。作为程序员，我们经常会发现自己需要一遍又一遍地重复同样的过程；在整个程序中重复重写相同的代码很快就变成了单调的时间和空间浪费。函数允许我们构建一个代码块，它可以在程序中的任意点被调用运行任意多次。

让我们通过创建一个简单的输出函数来看一个例子，这个函数无论何时被调用都会打印出“Hello readers”。

Python:

```
# Basic function that prints out hello world
def hello_world():    
    print("Hello readers")# Call to the hello_world function
hello_world()
```

退货:

```
$ python3 functions.py>> Hello readers
```

C++:

```
void printHelloWorld()
{   
    std::cout << "Hello readers!\n"; 
}// Call to the printHelloWorld function
printHelloWorld();
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> Hello readers!
```

## 函数的剖析

不管我们希望使用哪种语言，函数的要求都很简单。在 Python 中，必须遵循以下布局:

```
 |  1: All Python functions must begin by defining 
                   |     the block of code with def
1         2     3  |  2\. All Python functions must have a function 
def hello_world(): |     name; the function name should be all lower      
     4             |     case with words separated by an underscore
  return something |  3\. Parentheses should always follow the  
                   |     function name with a colon after
                   |  4\. All Python functions should end with a    
                   |     return statement or a print statement  
                   |     depending on the function’s use-case.
```

在 C++中，必须遵循以下布局:

```
 1          2      3
          void printHelloWorld()
          4
          {
                         5
              std::cout << "Hello readers!\n";
          }----------------------------------------------------------------
1: Like Python a function in C++ must be defined; however, C++ requires the programmer to specify what data type the function will be returned: void if we just want the function to print something   
out, int if we want a returned integer, string for string, double/float, etc.2: All C++ functions must have a function name; the function name should be in the form of snake-case where the first letter of the first word is lowercase and all proceeding words are capitalized with no space between.3\. Like Python the function name should be followed by parentheses the use of which will be discussed later in the article.4\. Where Python requires a colon to separate the function creation from the function’s code block, C++ requires the function’s code block to be wrapped between curly braces {} — remember the main function we are required to create in every C++ program we create.5\. Every C++ function must end with a print statement or by returning a data type that matches the function’s definition
```

当在函数的代码块中添加代码行时，不管使用什么编程语言，都应该缩进每一行代码。这有助于保持代码的可读性，实际上这也是 Python 编程语言所需要的——如果不这样做，就会抛出一条错误消息。

## 简单功能

我们将从看几个简单的例子开始进入函数。这些例子包括一个收集用户名的函数和一个打印用户名的函数。

我们收集用户名的函数将包括一个要求用户输入姓名的基本打印语句、一个保存用户输入的变量和一个允许在函数外部使用用户输入变量的返回语句。

Python:

```
# Function that gather's a user's name
def gather_user_name():
    print("What is your name?")
    name = input()
    return name# Save the results of calling gather_user_name into a variable
name = gather_user_name()
# Print out the user's name
print(f"Your name is {name}!")
```

退货:

```
$ python3 functions.py>> What is your name?
>> Daniel
>> Your name is Daniel!
```

C++:

```
std::string gatherUserName()
{
    std::string name;
    std::cout << "What is your name?\n";
    std::cin >> name;
    return name;
}
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> What is your name?
>> Daniel
>> Your name is Daniel!
```

我们打印用户名的函数将包括一个对前面函数的调用、一个保存返回的用户输入供这个新函数使用的变量，以及一个输出用户输入姓名的 print 语句。

Python:

```
# We can call a function within another function
def print_user_name():
    name = gather_user_name()
    print(f"Your name is {name}!")print_user_name()
```

退货:

```
$ python3 functions.py>> What is your name?
>> Daniel
>> Your name is Daniel!
```

C++:

```
void printUserNameByCalling()
{
    std::string name = gatherUserName();
    std::cout << "Your name is " << name << std::endl;
}printUserNameByCalling();
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> What is your name?
>> Daniel
>> Your name is Daniel!
```

## 带参数的函数

函数也可以包含必需的参数。这些信息必须在调用函数时传入，然后函数才能正常运行。放置顺序很重要，因为传递给函数调用的值将按照它们出现的顺序使用。我们将在这里看四个例子；一个只有一个参数的函数，打印出一个用户的名字；有两个参数的函数，返回两个数的和；返回两个数之差的双参数函数；以及一个有三个参数(其中一个有默认值)的函数，它打印出姓名、年龄和位置。

我们的第一个函数很简单，只需要传递一个参数(name)就可以运行，只需要一个 print 语句就可以在一个句子中打印出传入的参数。

Python:

```
# We can also pass in a parameter to a function for use within the function
def print_user_name(name):
    print(f"Your name is {name}!")name = gather_user_name()
print_user_name(name)
```

退货:

```
$ python3 functions.py>> What is your name?
>> Daniel
>> Your name is Daniel!
```

C++:

```
void printUserNameUsingParam(std::string name)
{
    std::cout << "Your name is " << name << std::endl;
}printUserNameUsingParam("Daniel");
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> Your name is Daniel
```

我们的第二个和第三个函数都需要传递两个参数(num1 和 num2)才能运行。这些函数中的每一个都只包含一行，一个返回函数二中两个参数之和的 return 语句和一个返回函数三中两个参数之差的 return 语句。

Python 函数 2:

```
# We can use multiple parameters within a function
def find_sum(num1, num2):
    return num1 + num2print(find_sum(5, 7))
```

退货:

```
$ python3 functions.py>> 12
```

C++函数 2:

```
int findSum(int num1, int num2)
{
    return num1 + num2;
}std::cout << findSum(5, 7) << std::endl;
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> 12
```

Python 函数 3:

```
def find_difference(num1, num2):
    return num1 - num2print(find_difference(7, 5))
```

退货:

```
$ python3 functions.py>> 2
```

C++函数 3:

```
int findDifference(int num1, int num2)
{
    return num1 - num2;
}std::cout << findDifference(7, 5) << std::endl;
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> 2
```

对于需要参数的函数，在进行函数调用时，可能会有一个值更频繁地重复出现，或者我们需要一个值来指示没有给定值传递给特定的参数；这就是默认值发挥作用的地方。当定义一个函数的参数时，我们可以用一个缺省值实例化那个参数，如果一个新值没有被传递到函数的调用中，这个缺省值将被自动设置。使用具有给定默认值的参数时，该参数必须放在所有不包含默认值的参数之后。以下示例包括三个参数，其中一个被赋予默认值。函数体包括三个打印语句，一个打印姓名，一个打印年龄，最后一个打印位置。

Python:

```
def print_info(name, age, location="Unknown"):
    print(f"Hello, your name is {name}...")
    print(f"You are {age} years old...")
    print(f"and you are from {location}...")print_info("Daniel", 32)
print_info("Daniel", 32, "Utah")
```

退货:

```
$ python3 functions.py>> Hello, your name is Daniel...
>> You are 32 years old...
>> and you are from Unknown...
>> Hello, your name is Daniel
>> You are 32 years old...
>> and you are from Utah
```

C++:

```
void printInfo(std::string name, int age, std::string location="unknown")
{
    std::cout << "Hello, your name is " << name << std::endl;
    std::cout << "You are " << age << " years old...\n";
    std::cout << "and you are from " << location << std::endl;
}printInfo("Daniel", 32);
printInfo("Daniel", 32, "Utah");
```

退货:

```
$ g++ -o functions functions.cpp
$ ./functions>> Hello, your name is Daniel
>> You are 32 years old...
>> and you are from unknown
>> Hello, your name is Daniel
>> You are 32 years old...
>> and you are from Utah
```

亲爱的读者们，感谢你们加入我的 Python 到 C++的另一期文章。我希望下次我们深入课堂的时候能见到你。一如既往地保持回复和快乐编码。