# Python 到 C++，数据科学家学习新语言的旅程——使用数字和字符串

> 原文：<https://medium.com/geekculture/python-to-c-a-data-scientists-journey-to-learning-a-new-language-working-with-numbers-and-3a14c468079d?source=collection_archive---------55----------------------->

![](img/890736ea088de4cf94031ae66343a512.png)

# **简介**

读者，欢迎回到 Python 到 C++系列的下一期。在上一次冒险中，我们学习了输出方法、数据类型、变量，以及收集 C++和 Python 中的用户输入。在这一卷中，我们将在我们感兴趣的两种语言中学习数字和字符串。我们将涉及数字与数学运算符的使用，以及访问字符串中的单个字符或字符组。

在下面的代码片段中使用的原始代码可以在库[这里](https://github.com/Daniel-Benson-Poe/Python_to_Cpp_Series/tree/main/Article_3)找到。

# **与数字打交道**

在 Python 中，数字的使用非常简单，只需输入用于整数数据类型的数字，并包含用于双精度数据类型的小数。这可以通过下面简单的数字 13 的打印输出在代码中看到:

```
# Simple number printout
print(13)
```

使用终端:

```
$ python3 working_with_numbers.py>> 13
```

在 C++中使用了相同的方法，如下面的代码所示:

```
// # Simple number printout
std::cout << 13 << std::endl;
```

导致输出:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 13
```

就像在数学中一样，Python 和 C++中的数字可以通过数学运算来操作；计算机被告知通过特定的操作符来执行这些操作。这些运算包括:加、减、乘、除。

**加法:**在 Python 和 C++中，指示计算机执行加法的操作符是“+”符号。将这个符号放在两个数之间，将告诉计算机对两个给定的数进行加法运算；由于加法性质，哪个数字放在运算符前面，哪个数字放在运算符后面，都不会改变最终答案。在下面的代码中可以看到一个例子:

```
print("1 + 1: ", 1 + 1)
print("1.5 + 1.5: ", 1.5 + 1.5)
print("1.5 + 1: ", 1.5 + 1)
a = 1
b = 1.5
print("a (1) + a (1): ", a + a)
print("a (1) + b (1.5): ", a + b)
```

退货:

```
$ python3 working_with_numbers.py>> 1 + 1:  2
>> 1.5 + 1.5:  3.0
>> 1.5 + 1:  2.5
>> a (1) + a (1):  2
>> a (1) + b (1.5):  2.5
```

对于 C++:

```
std::cout << "1 + 1: " << 1 + 1 << std::endl;
std::cout << "1.5 + 1.5: " << 1.5 + 1.5 << std::endl;
std::cout << "1.5 + 1: " << 1.5 + 1 << std::endl;
int a = 1;
double b = 1.5;
std::cout << "a (1) + a (1): " << a + a << std::endl;
std::cout << "a (1) + b (1.5): " << a + b << std::endl;
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 1 + 1: 2
>> 1.5 + 1.5: 3
>> 1.5 + 1: 2.5
>> a (1) + a (1): 2
>> a (1) + b (1.5): 2.5
```

**减法:**指示计算机执行减法的通用运算符是“-”符号。将这个符号放在两个数字之间会告诉计算机从第一个数字中减去第二个数字；这里数字的位置很重要。以下代码给出了一个示例:

```
print("1 - 1: ", 1 - 1)
print("1.5 - 1: ", 1.5 - 1)
print("a (1) - a (1): ", a - a)
print("b (1.5) - a (1): ", b - a)
```

退货:

```
$ python3 working_with_numbers.py>> 1 - 1:  0
>> 1.5 - 1:  0.5
>> a (1) - a (1):  0
>> b (1.5) - a (1):  0.5
```

在 C++中:

```
std::cout << "1 - 1: " << 1 - 1 << std::endl;
std::cout << "1.5 - 1: " <<  1.5 - 1 << std::endl;
std::cout << "a (1) - a (1): " <<  a - a << std::endl;
std::cout << "b (1) - a (1): " <<  b - a << std::endl;
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 1 - 1: 0
>> 1.5 - 1: 0.5
>> a (1) - a (1): 0
>> b (1) - a (1): 0.5
```

**乘法:**告诉计算机将两个数相乘，使用的运算符是“*”；Python 和 C++都是这种情况。像加法一样，乘法具有乘法性质，无论哪个数字放在运算符前面，哪个放在运算符后面，最终答案都保持不变。代码中乘法运算的一个示例如下:

```
print("5 * 10: ", 5 * 10)print("2.5 * 5.5: ", 2.5 * 5.5)
print("2.5 * 10: ", 2.5 * 10)
a = 5
b = 10
c = 2.5
print("a (5) * b (10): ", a * b)
print("c (2.5) * c (2.5): ", c * c)
```

退货:

```
$ python3 working_with_numbers.py>> 5 * 10:  50
>> 2.5 * 5.5:  13.75
>> 2.5 * 10:  25.0
>> a (5) * b (10):  50
>> c (2.5) * c (2.5):  6.25
```

在 C++中:

```
std::cout << "5 * 10: " << 5 * 10 << std::endl;
std::cout << "2.5 * 5.5: " <<  2.5 * 5.5 << std::endl;
std::cout << "2.5 * 10: " <<  2.5 * 10 << std::endl;
int c = 5;
int d = 10;
double e = 2.5;
std::cout << "c (5) * d (10): " <<  c * d << std::endl;
std::cout << "e (2.5) * e (2.5): " <<  e * e << std::endl;
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 5 * 10: 50
>> 2.5 * 5.5: 13.75
>> 2.5 * 10: 25
>> c (5) * d (10): 50
>> e (2.5) * e (2.5): 6.25
```

除法:除法运算是事情开始变得有点棘手的地方。除法本质上是一种通过分数运算的运算。因此，我们经常会发现自己在处理双精度或浮点数，因为计算机对其指令的精确度非常挑剔，所以要求整数商与要求双精度或浮点数商需要不同的方法。

在 Python 中，我们使用不同的运算符符号来表示我们是想要整数还是浮点数答案。为了返回一个浮点数，我们使用“/”符号嵌套在两个我们想要求其商的数之间；像减法一样，除法也依赖于数字的位置。下面的代码就是一个很好的例子:

```
print("10 / 2: ", 10/2)
print("14 / 3: ", 14/3)
print("12.4 / 2.2: ", 12.4/2.2)
a = 120
b = 13
c = 45
print("a (120) / b (13): ", a/b)
print("c (45) / b (13): ", c/b)
print(" a (120) / c (45): ", a/c)
```

退货:

```
$ python3 working_with_numbers.py>> 10 / 2:  5.0
>> 14 / 3:  4.666666666666667
>> 12.4 / 2.2:  5.636363636363636
>> a (120) / b (13):  9.23076923076923
>> c (45) / b (13):  3.4615384615384617
>> a (120) / c (45):  2.6666666666666665
```

如果我们想求一个整数的商，或者换句话说，对两个数进行除法运算并得到一个整数的答案，我们在两个数之间使用“\”符号。计算机将执行除法计算，丢弃接收到的任何小数，并返回没有任何舍入的整数。我们可以在下面的代码中看到这一点:

```
print("10 // 2: ", 10//2)
print("14 // 3: ", 14//3)
print("12.4 // 2.2: ", 12.4//2.2)
print("a (120) // b (13): ", a//b)
print("c (45) // b (13): ", c//b)
print("a (120) // c (45): ", a//c)
```

退货:

```
$ python3 working_with_numbers.py>> 10 // 2:  5
>> 14 // 3:  4
>> 12.4 // 2.2:  5.0
>> a (120) // b (13):  9
>> c (45) // b (13):  3
>> a (120) // c (45):  2
```

在 C++中，我们使用不同的数据类型来告诉计算机它应该返回一个整数还是一个浮点数作为最终的商。相同的运算符“/”符号用于两种除法情况。为了告诉计算机我们需要浮点返回，我们必须确保运算符前后至少有一个数字是浮点或双精度数据类型。C++将自动返回一个与该数据类型匹配的商。这可以在下面的代码中看到:

```
std::cout << "10.0 / 2: " << 10.0/2 << std::endl;
std::cout << "14 / 3.0: " <<  14/3.0 << std::endl;
std::cout << "12.4 / 2.2: " <<  12.4/2.2 << std::endl;
double f = 120;
double g = 13;
double h = 45;
std::cout << "f (120.0) / g (13.0): " << f/g << std::endl;
std::cout << "h (45.0) / g (13.0): " << h/g << std::endl;
std::cout << "f (120.0) / h (45.0): " << f/h << std::endl;
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 10.0 / 2: 5
>> 14 / 3.0: 4.66667
>> 12.4 / 2.2: 5.63636
>> f (120.0) / g (13.0): 9.23077
>> h (45.0) / g (13.0): 3.46154
>> f (120.0) / h (45.0): 2.66667
```

为了告诉计算机我们希望商作为一个整数返回，我们只需确保除法运算符前后的数字都是整数数据类型。计算机将执行除法运算，去掉任何余数，返回任何整数，不进行舍入。在下面的代码中可以看到这样一个例子:

```
std::cout << "10 / 2: " <<  10/2 << std::endl;
std::cout << "14 / 3: " <<  14/3 << std::endl;
int i = 120;
int j = 13;
int k = 45;
std::cout << "i (120) / j (13): " <<  i/j << std::endl;
std::cout << "k (45) / j (13): " << k/j << std::endl;
std::cout << "i (120) / k (45): " << i/k << std::endl;
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 10 / 2: 5
>> 14 / 3: 4
>> i (120) / j (13): 9
>> k (45) / j (13): 3
>> i (120) / k (45): 2
```

**模:**模是计算机科学中的一种特殊运算。它类似于除法运算，只是它的返回实际上是该运算的商的余数。例如，如果我们用 3 除以 2，我们知道我们的商是 1，余数是 1。模告诉计算机只返回那个余数，而把其他的都扔掉。在 Python 和 C++中，告诉计算机执行取模运算的运算符是由我们希望对其执行运算的两个数之间的“%”符号表示的。在下面的代码中可以看到这样一个例子:

```
print("10 % 2: ", 10%2)
print("14 % 3: ", 14%3)
print("12.4 % 2.2: ", 12.4%2.2)
print("a (120) % b (13): ", a%b)
print("c(45) % b (13): ", c%b)
print("a (120) % c (45): ", a%c)
```

退货:

```
$ python3 working_with_numbers.py>> 10 % 2:  0
>> 14 % 3:  2
>> 12.4 % 2.2:  1.3999999999999995
>> a (120) % b (13):  3
>> c(45) % b (13):  6
>> a (120) % c (45):  30
```

在 C++中:

```
std::cout << "10 % 2: " << 10%2 << std::endl;
std::cout << "14 % 3: " <<  14%3 << std::endl;
// std::cout << "10 % 2: " <<  12.4%2.2 << std::endl; cannot use modulo operand with floating point values in c++, only integers
std::cout << "i (120) % j (13): " << i%j << std::endl;
std::cout << "k (45) % j (13): " << k%j << std::endl;
std::cout << "i (120) % k (45): " << i%k << std::endl;
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 10 % 2: 0
>> 14 % 3: 2
>> i (120) % j (13): 3
>> k (45) % j (13): 6
>> i (120) % k (45): 30
```

请注意，虽然在 Python 中我们可以使用浮点数来求模，但在 C++中我们不能使用浮点数来求模。

**偶数/奇数示例:**模运算的一个很大的用途是当我们希望确定一个数是偶数还是奇数时。当我们稍后讨论条件语句时，这一点的重要性将变得更加明显，但目前我们知道，在许多情况下，程序员确定一个特定的数字或变量是偶数还是奇数是很重要的。使用模运算，我们可以将问题中的数字放在“%”运算符前面，将数字 2 放在运算符后面，然后看看余数(我们的返回值)是多少。如果返回值是 0，这意味着所讨论的数字是偶数；如果返回值是 1，这意味着所讨论的数字是奇数。下面的代码给出了这样的例子:

```
print("110 % 2: ", 110%2)
print("111 % 2: ", 111%2)
a = 2
print("8765 % a (2): ", 8765%a)
print("86312 % a (2): ", 86312%a)
```

退货:

```
$ python3 working_with_numbers.py>> 110 % 2:  0
>> 111 % 2:  1
>> 8765 % a (2):  1
>> 86312 % a (2):  0
```

和 C++:

```
std::cout << "110 % 2: " << 110%2 << std::endl;
std::cout << "111 % 2: " <<  111%2 << std::endl;
int l = 2;
std::cout << "8765 % l (2): " <<  8765%l << std::endl;
std::cout << "86312 % l (2): " <<  86312%l << std::endl
```

退货:

```
$ g++ -o working_with_numbers working_with_numbers.cpp
$ ./working_with_numbers>> 110 % 2: 0
>>  111 % 2: 1
>> 8765 % l (2): 1
>> 86312 % l (2): 0
```

当然，这是一种可以用来确定除数的方法；我们讨论的数字能被 3 整除吗？4 呢？5?当我们学习条件句的用法时，这一点的重要性会变得更加明显。

# **使用琴弦**

字符串由组合在一起的字符组成:

```
print("This string has 36 characters in it.")
print("String length: ", len("This string has 36 characters in it."))
```

退货:

```
python3 working_with_strings.py>> This string has 36 characters in it.
>> String length:  36
```

在 Python 和 C++中，我们可以使用“+”运算符将字符串追加在一起，如以下代码所示:

```
a = "Hello"
b = " readers!"
print(a + b)
```

退货:

```
$ python3 working_with_strings.py>> Hello readers!
```

在 C++中:

```
std::string a = "Hello";
std::string b = " readers!";
std::cout << a + b << std::endl;
```

退货:

```
$ g++ -o working_with_strings working_with_strings.cpp
$ ./working_with_strings>> Hello readers!
```

字符串中的每个字符都属于该字符串中的一个特定位置。这些位置称为索引(单数:index)。每个位置或索引由一个唯一的数字表示。任何字符串中的起始位置(即索引)都是 0，之后每个位置递增 1。例如，字符串“Hello”在索引 0 处有一个“H”，在索引 4 处有一个“o”。

如果我们知道它们的索引位置，我们就可以访问字符串中的特定字符！例如，字符串“Hello”的索引 0 是“H”:

```
print(a[0])
```

退货:

```
$ python3 working_with_strings.py>> H
```

在 C++中:

```
std::cout << a[0] << std::endl;
```

退货:

```
$ g++ -o working_with_strings working_with_strings.cpp
$ ./working_with_strings>> H
```

我们可以通过调用该字符串的索引 4 来访问“Hello”中的“o ”:

```
print(a[4])
```

退货:

```
$ python3 working_with_strings.py>> o
```

在 C++中:

```
std::cout << a[4] << std::endl;
```

退货:

```
$ g++ -o working_with_strings working_with_strings.cpp
$ ./working_with_strings>> o
```

还可以访问字符串中的一组特定字符。然而，这在 Python 和 C++中是不同的。在 Python 中，如果我们有字符串“readers！”我们可以通过调用索引 1–4 来访问字符分组“read”(注意，在索引 0 处我们有一个空格，它仍然被算作占据该索引的字符):

```
print(b[1:5])
```

退货:

```
$ python3 working_with_strings.py>> read
```

请注意，名为 1 的第一个索引是包含性的，而名为 5 的第二个索引是不包含性的。这意味着将访问从索引 1 到索引 5 的字符，但不包括索引 5。

我们甚至可以从字符串的末尾开始这样做，即使我们不知道字符串的长度。这是通过使用-1 作为索引来实现的，它总是用于指示字符串的最后一个索引:

```
print(b[-1])
```

退货:

```
$ python3 working_with_strings.py>> !
```

使用负索引允许我们从字符串的末尾开始访问特定的字符。在这种情况下，索引每减少一个字符，我们就向字符串的开头移动一个字符。对于字符串“读者！”索引-1 给了我们'！'索引 2 给我们“s ”,索引 3 给我们“r ”:

```
print(b[-1])
print(b[-2])
print(b[-3])
```

退货:

```
$ python3 working_with_strings.py>> !
>> s
>> r
```

我们还可以使用负索引来访问特定的字符分组。对于字符串“读者！”：

```
print(b[-5:-1])
print(b[-5:])
```

返回:

```
$ python3 working_with_strings.py>> ders 
>> ders!
```

请注意，省略第二个数字相当于说“我想要从第一个指示的索引到字符串末尾的所有字符。”类似地，省略第一个索引号相当于说“我想要从字符串开头开始的所有字符，但不包括给定的索引号。去掉第一个和第二个数字(只包括':')意味着我们需要字符串开头到结尾的所有字符:

```
print(a[1:])
print(a[:2])
print(b[:])
```

退货:

```
$ python3 working_with_strings.py>> ello
>> He
>>  readers!
```

在 C++中，访问字符串中的特定字符组的方式略有不同。在 C++中，使用程序顶部的#include <string>调用程序中的 string 类，从传递给给定变量的字符中构建一个数组。例如，用字符串“Hello”初始化的变量 a 是一个幕后数组，由字符['H']，['e']，['l']，['l']，['o']组成。为了从一个字符串中访问多个字符，我们创建了一个新变量，我们将把分组的字符初始化到这个变量中:</string>

```
std::string c(b, 1, 4); // where c is the new variable, b is the variable we are accessing, 1 is the starting index and 4 is how many indices we are including
std::cout << c << std::endl;
std::cout << c.length() << std::endl;
```

退货:

```
$ g++ -o working_with_strings working_with_strings.cpp
$ ./working_with_strings>> read
>> 4
```

我们也可以通过一些创意在 C++中访问一个字符串的最后一个字符。我们可以使用想要访问的字符串的长度作为起始索引。记住将长度减一以访问正确的索引(字符串的长度总是比可访问的索引数多 1):

```
std::string d(b, b.length()-1, 1);
std::cout << d << std::endl;
std::cout << d.length() << std::endl;std::string e(b, b.length()-2, 1);
std::cout << e << std::endl;
std::cout << e.length() << std::endl;std::string f(b, b.length()-3, 1);
std::cout << f << std::endl;
std::cout << f.length() << std::endl;
```

退货:

```
$ g++ -o working_with_strings working_with_strings.cpp
$ ./working_with_strings>> !
>> 1
>> s
>> 1
>> r
>> 1
```

更具创造性的是，我们可以通过将组方法调用中传递的每个数字加 1 来访问字符串的最后 n 个字符。下面的代码给出了一个更清晰的例子:

```
std::string e(b, b.length()-2, 2);
std::cout << e << std::endl;
std::cout << e.length() << std::endl;std::string f(b, b.length()-3, 3);
std::cout << f << std::endl;
std::cout << f.length() << std::endl;std::string g(b, b.length()-4, 4);
std::cout << g << std::endl;
std::cout << g.length() << std::endl;
```

退货:

```
$ g++ -o working_with_strings working_with_strings.cpp
$ ./working_with_strings>> s!
>> 2
>> rs!
>> 3
>> ers!
>> 4
```

亲爱的读者们，感谢你们再次加入我的 Python 到 C++的这一期！我希望你学到了很多，下次当我们讨论 Python 和 C++中的条件语句并使用这两种语言创建我们自己的基本计算器时，我们会再见面。一如既往地重复它，快乐编码！