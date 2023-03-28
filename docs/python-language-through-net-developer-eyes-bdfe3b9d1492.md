# Python 语言通过。网络开发者的眼睛

> 原文：<https://medium.com/geekculture/python-language-through-net-developer-eyes-bdfe3b9d1492?source=collection_archive---------14----------------------->

## 值得记住的关于 Python 通用场景的有用代码片段和主题列表

如果你在成为一名 C#开发人员很长一段时间后才开始学习 Python，你可能希望拥有与 C#相同的工具集。这里我想给出一种从 C#到 Python 的映射，尽管我应该承认这并不准确。我使用 Python 开发时陷入困境的时刻作为本文的要点。值得一提的是，这不是 C#和 Python 在任何方面的技术比较。所以把这篇文章当成小说来读，它会告诉你两种语言的一个快速概览，它们有什么相似之处，又有什么不同之处。这份名单并不完整。

![](img/8b2c8ed0e4f140d952ffdbd702e37e6c.png)

## 1.Python 中的多重继承

与 C#相反，Python 有多重继承，类似于 C++。就个人而言，我看不出多重继承有多大用处，为了代码清晰起见，我甚至建议避免使用多重继承。但是如果你决定使用它，请记住与这种继承相关的“[钻石问题](https://en.wikipedia.org/wiki/Multiple_inheritance#The_diamond_problem)”。至于 Python，他们有明确的方法解析顺序(MRO)。

```
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

> 解释语义所需的唯一规则是用于类属性引用的解析规则。这是深度优先，从左到右。因此，如果在 DerivedClassName 中找不到属性，则在 Base1 中搜索，然后(递归地)在 Base1 的基类中搜索，只有在那里找不到时，才在 Base2 中搜索，依此类推。

 [## 多重继承

### Python 也支持有限形式的多重继承。具有多个基类的类定义看起来像…

docs.python.org](https://docs.python.org/release/1.5.1p1/tut/multiple.html) 

还有一些更高级的例子:

[](/technology-nineleaps/python-method-resolution-order-4fd41d2fcc) [## Python 方法解析顺序

### 在 Python 中，一个类可以从多个类继承特性和属性，从而实现多重继承…

medium.com](/technology-nineleaps/python-method-resolution-order-4fd41d2fcc) 

## 2.Python Lambdas

C#和 Python 中的 Lambdas 代表匿名函数。语法稍有不同，您可以对它们做同样的事情，包括作为参数传递给另一个函数和在闭包中使用。这是它们在 C#中的样子:

```
var a = 3;
Func<int, int> f1 = x => a + x;
Func<double, Func<int, int>, double> f2 = (y, f) => y / f((int)y);
var result = f2(5, f1);
Console.WriteLine($"result: {result}"); # output "result: 0.625"
```

在 Python 中:

```
a = 3
f1 = lambda x: a + x
f2 = lambda x, f: x / f(x)
result = f2(5, f1)
print("result:", result)  # output "result: 0.625"
```

如果 lambda 没有参数，它们就省略了:

```
f = lambda: 5
f()  # returns 5
```

[](https://realpython.com/python-lambda/) [## 如何使用 Python Lambda 函数——真正的 Python

### 参加测验“Python 和其他编程语言中的 lambda 表达式源于 Lambda 演算，是一种……

realpython.com](https://realpython.com/python-lambda/) 

## 3.关闭

Python 和 C#有闭包。除了语法不同，它们的用法没有太大的区别。

C#闭包:

```
Func<int, Action> create_closure = delegate(int a){
    Action f = () => Console.WriteLine($"this is: {a}");
    return f;
};var f1 = create_closure(1);
var f2 = create_closure(2);f1();
f2();
```

Python 闭包:

```
def create_closure(a):

    def f():
        print("this is:", a)

    return f

f1 = create_closure(1)
f2 = create_closure("text")f1()
f2()
```

## 4.部分功能

C#中没有这样的东西。但是如果你懂 C++的话，你会很熟悉的。我举个小例子:

```
from functools import partialdef f1(a,b,c):
    return 100*a+10*b+c

f2 = partial(f1, b=5, c=2)
print(f2(3))
```

函数`f2`是一个局部函数。您可能会注意到，它是函数`f1`的一种包装，带有两个其他参数的硬编码值。在 C#中，我们可以通过编写调用另一个函数的附加函数来实现类似的功能。

```
Func<int, int, int, int> f1 = (a, b, c) => 100 * a + 10 * b + c;
Func<int, int> f2 = (a) => f1(a, 5, 2);
Console.WriteLine($"{f2(3)}");
```

 [## functools -可调用对象上的高阶函数和操作- Python 3.9.5 文档

### 源代码:Lib/functools.py 该模块用于高阶函数:作用于或返回其他函数的函数…

docs.python.org](https://docs.python.org/3/library/functools.html) 

## 5.Python 中的理解

当你用 C#写一个需要迭代某些集合(列表、数组、字典)的项目的代码时，你会认为这个任务属于循环操作符的范畴。这是你从学习 Python 开始就需要知道的:它有一种更高效的方式来完成这样的任务。这叫做列表理解或字典理解。

[](https://switowski.com/blog/for-loop-vs-list-comprehension) [## 对于循环和列表理解

### Python 中许多简单的“for 循环”可以用列表理解来代替。你经常可以听到列表理解…

switowski.com](https://switowski.com/blog/for-loop-vs-list-comprehension) 

让我给你一个简单任务的例子:过滤一个数组。在 C#中，它看起来像这样:

```
var a = new [] {1, 2, 3, 4, 5, 6, 7, 8, 9};
var result = new List<int>();
foreach (var item in a){
    if (item % 2 == 0){
        result.Add(item);
    }
}
foreach(var item in result){
    Console.Write($"{item}, ");
}
```

我们可以通过使用 Linq 来优化 C#中的这段代码，但实际上它仍然是一个循环操作符。

```
var a = new [] {1, 2, 3, 4, 5, 6, 7, 8, 9};
var result = a.Where(item => item % 2 == 0);
foreach(var item in result){
    Console.Write($"{item}, ");
}
```

对于 Python 的例子，我想给出两个选项的例子以及一个性能检查，你可以在[在线 Python 编辑器](https://www.programiz.com/python-programming/online-compiler/)中尝试。有理解的版本总是快一点。

```
import timeita = range(10000)  # a big listdef filtering_using_comprehension():
    result = [i for i in a if i%2==0]

def filtering_using_loop():
    result2 = []
    for i in a:
        if i%2==0:
            result2 += [i]

n = 100        
print("avg.time:", timeit.timeit(filtering_using_comprehension, number=n)/n*1000,"ms")
print("avg.time:", timeit.timeit(filtering_using_loop, number=n)/n*1000,"ms")output:
avg.time: 0.7362717300020449 ms
avg.time: 1.1933808300091187 ms
```

当你使用字典时，理解技巧会变得很方便。下面是一个简单的代码，它使用 map-reduce 技术和字典理解来计算随机文本中的字符数:

```
from functools import reduceinput_text = "aabcccbaabc"c = [(c, 1) for c in input_text]
d = {c: 0 for c in input_text}
r = {k: reduce(lambda x, y: x+y, [i[1] for i in list(filter(lambda x: x[0]==k, c))]) for k in d}
print(r)output:
{'a': 4, 'b': 3, 'c': 4}
```

[](https://realpython.com/list-comprehension-python/) [## 在 Python 中何时使用列表理解——真正的 Python

### 有几种不同的方法可以在 Python 中创建列表。为了更好地理解使用列表的利弊…

realpython.com](https://realpython.com/list-comprehension-python/) 

## 6.测量函数执行的时间长度

我知道有两种方法可以做到这一点。一个类似于我们平时在 C#中通过秒表类做的事情。在 Python 中有一个类似用法的计时器类。

[](https://realpython.com/python-timer/) [## Python 定时器函数:监控代码的三种方法——真正的 Python

### 在这个循序渐进的教程中，您将学习如何使用 Python 定时器函数来监控您的程序有多快…

realpython.com](https://realpython.com/python-timer/) 

另一种方法是使用专门为这种测量和分析开发的模块:`timeit`。

 [## timeit -测量小代码片段的执行时间- Python 3.9.5 文档

### 源代码:Lib/timeit.py 这个模块提供了一种简单的方法来为小部分 Python 代码计时。它既有…

docs.python.org](https://docs.python.org/3/library/timeit.html) 

## 7.Python 中的循环运算符可以有 Else 部分

如果您需要知道一个循环操作符是否在检查所有项目之前退出，Else 部分就派上了用场。在 C#中，我们没有这样的东西，做这样的检查会引入额外的标志。

当循环语句正常完成时，将执行 Else 部分。如果你的代码遇到`break`或者`return`操作符，就不会执行。

 [## 21.for/else - Python 提示 0.1 文档

### 常见的构造是运行一个循环并搜索一个项目。如果找到了该项，我们就使用…

book.pythontips.com](https://book.pythontips.com/en/latest/for_-_else.html) 

## 8.Python 中的链接比较运算符

值得一提的是，这样的语法在 Python 中是可能的:`a < b < c`。

[](https://www.geeksforgeeks.org/chaining-comparison-operators-python/) [## Python - GeeksforGeeks 中的链接比较运算符

### 在编程语言中，检查两个以上的条件是很常见的。假设我们要检查以下条件:a…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/chaining-comparison-operators-python/) 

## 9.打包和解包参数，*args，*kwargs

在 C#中，我们可以使用`params`关键字来指定一个方法接受可变数量的参数。我们还可以使用带有默认值的命名参数，使它们的用法可选。

Python 有更灵活的语法来实现类似的行为。在 Python 中，当一个方法接收有限数量的参数时，可以解包一个数组，将其值作为方法参数的值传递。

```
def f1(a, b, c, d):
    print(a,b,c,d)  # single values: 1 2 3 9a=[1,2,3]  
f1(*a,9)
```

如果一个方法接收可变数量的参数(C#中使用`params`的确切情况)，你可以打包位置参数。

```
def f2(*args):
    print(args)  # a tuple: (1, 2, 't')
    print(args[1])  # output: 2

f2(1,2,"t")
```

与 C#不同，Python 可以打包命名参数。

```
def f3(**kwargs):
    print(kwargs)  # a dictionary: {'a': 1, 'value': 2, 'name': 't'}
    print(kwargs["name"])  # output: t

f3(a=1,value=2,name="t")
```

[](https://www.geeksforgeeks.org/packing-and-unpacking-arguments-in-python/) [## 在 Python - GeeksforGeeks 中打包和解包参数

### 我们使用两个操作符*(用于元组)和**(用于字典)。背景考虑这样一种情况，我们有一个函数…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/packing-and-unpacking-arguments-in-python/) [](https://www.geeksforgeeks.org/args-kwargs-python/) [## * Python 中的 args 和* * kwargs-GeeksforGeeks

### 在 Python 中，我们可以使用特殊符号向函数传递可变数量的参数。有两个特别的…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/args-kwargs-python/) 

## 10.Python 中的生成器函数

当您处理大量数据(大于 RAM 所能分配的容量)或重复的 web 请求时，以及当您需要在代码获得控制权后立即处理每一个结果时，C#中有一个特性非常有用。操作符`yield return`与`IEnumerable<T>`一起允许这样做，一个简单的例子如下:

```
public static IEnumerable<int> Power(int number, int exponent)
{
    int result = 1;
    for (int i = 0; i < exponent; i++)
    {
        result = result * number;
        yield return result;
    }
}
```

在 Python 中，这样的函数被称为生成器。与 C#相比，这样的函数只使用`yield`，它取代了`return`操作符。

```
def power(number, exponent):
    result = 1
    i = range(exponent)
    for n in i:
        result = result * number
        yield result
```

 [## 生成器- Python Wiki

### 生成器函数允许你声明一个行为像迭代器的函数，也就是说，它可以在 for 循环中使用…

wiki.python.org](https://wiki.python.org/moin/Generators) 

## 11.Python 中的 Async/Await 和从非异步运行异步代码

这是当你开始使用 Python 中的 IO 绑定操作(数据库、API 等)时出现的第一个问题。).IMO 实例胜于文字，所以下面是一个简单的代码，它启动几个并行任务，然后等待它们全部完成。

```
import asyncio
from time import timedef run_async(async_func):
    loop = asyncio.get_event_loop()
    result = loop.run_until_complete(async_func)
    return resultasync def delay(sleep):
    print(f'before: {sleep}')
    await asyncio.sleep(sleep)
    print(f'after: {sleep}')async def parallel_tasks_with_await_all():
    tasks = []
    task = asyncio.create_task(delay(2))
    tasks.append(task)
    task = asyncio.create_task(delay(5))
    tasks.append(task)
    task = asyncio.create_task(delay(3))
    tasks.append(task)
    await asyncio.gather(*tasks, return_exceptions=True)async def sync_block():
    await delay(2)
    await delay(5)
    await delay(3)print("ready")
start = time()
run_async(parallel_tasks_with_await_all())
print(f"Executed for {time()-start} secs")
print("finish")The output:
ready
before: 2
before: 5
before: 3
after: 2
after: 3
after: 5
Executed for 5.002882242202759 secs
finish
```

此示例显示了几个特征:

*   方法`run_async`是在非异步上下文中调用异步函数的一个例子。在 Python 中，这可能是有用的，因为一些工具和框架(如 Airfow)不使用 async/await，但同时您希望编写能够受益于 async 代码的库。
*   方法`parallel_tasks_with_await_all`展示了如何启动几个任务，然后一起等待它们(类似于 C#中的`Task.WaitAll`)。
*   方法`sync_block`以同步的方式显示了方法的非阻塞执行。

您可以通过调用方法`sync_block`而不是`parallel_tasks_with_await_all`来检查 Python 是否并行执行任务:

```
old: run_async(parallel_tasks_with_await_all())new: run_async(sync_block())The output:
ready
before: 2
after: 2
before: 5
after: 5
before: 3
after: 3
Executed for 10.010915279388428 secs
finish
```

还可以控制并行执行的任务数量。Python 模块`asyncio`有信号量的实现。以下代码片段显示了如何使用它来限制任务数量:

```
async def gather_with_concurrency(n, *tasks):
    semaphore = asyncio.Semaphore(n)

    async def sem_task(task):
        async with semaphore:
            return await task
    return await asyncio.gather(*(sem_task(task) for task in tasks))
```

[](https://www.integralist.co.uk/posts/python-asyncio/) [## 用 Asyncio ⋆马克麦克唐奈的 Python 编写并发性指南

### 这是 Python 的 asyncio 模块的快速指南，基于 Python 3.8 版。简介为什么关注 asyncio…

www.integralist.co.uk](https://www.integralist.co.uk/posts/python-asyncio/) [](https://dev.to/0xbf/turn-sync-function-to-async-python-tips-58nn) [## 将同步功能转变为异步 Python 技巧

### 我们可以写一个函数包装一个同步函数一个异步函数:如果我们使用同步函数

开发到](https://dev.to/0xbf/turn-sync-function-to-async-python-tips-58nn)