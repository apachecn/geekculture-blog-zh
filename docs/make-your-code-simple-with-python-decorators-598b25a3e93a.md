# Python:用装饰器简化代码

> 原文：<https://medium.com/geekculture/make-your-code-simple-with-python-decorators-598b25a3e93a?source=collection_archive---------16----------------------->

![](img/2a3a4aed14f6355b763c0d1d98fe4723.png)

# 什么是装修工？

装饰器是一种在不改变代码的情况下改变或修改函数或类方法的 T2 行为的方法。

那些装饰者**非常强大**并且有不同的用例，我们将会看到其中的一些。

但是在此之前，让我们快速回顾一下 python 的基础知识。

# Python 中函数是如何工作的？

## 简单功能:

首先，函数是一个**对象**。像任何其他对象一样，函数也有属性和方法。

```
def my_function():
    print("Decorators example")

my_function()
```

这个例子是一个“my_function”函数调用，它打印了“Decorators example”。

## 函数是一个对象:

但是如果我们显示“my_function”的函数签名，我们得到:

```
print(my_function)
$> <function my_function at 0x000001C94E156A68>
```

我们可以看到，我们在内存地址“0x000001C94E156A68”处获得了一个名为“my_fonction”的**函数对象**。

## 我们可以像使用其他变量一样使用函数:

这意味着，如果一个函数是一个对象，你可以在你的程序中**共享**或者**传递**。这样，我们可以将每个**函数签名**存储到一个字典中。

```
def my_function_type_1():
    print("type1")

def my_function_type_2():
    print("type2")

def my_function_type_3():
    print("type3")

types = {
    'type1': my_function_type_1,
    'type2': my_function_type_2,
    'type3': my_function_type_3
}

def typeHandler(type):
    types[type]()

typeHandler('type1')
typeHandler('type2')
typeHandler('type3')$> type1
$> type2
$> type3
```

基于字典键，如果情况，你可以调用相关的函数而不需要创建任何**。**

这使得你的代码更容易阅读和维护。

## 作为参数的函数:

你也可以传递一个函数作为**参数**。

```
def my_callback_function():
    print("Callback")

def my_function(callback):
    print("Doing my stuff")
    # when my logic is done call the parameter function
    callback()
    print("Callback done")

my_function(my_callback_function)$> Doing my stuff
$> Callback
$> Callback done
```

# 包装材料

在 python 中，可以在函数内部创建函数。

## 函数中的函数:

例如，如果您想创建一个函数，让**计算另一个函数的时间**:

```
import datetime as dt

def time_compute(compute_function):
    def wrap():
        begin = dt.datetime.utcnow()
        compute_function()
        end = dt.datetime.utcnow()
        print("Time: ", end - begin)

    wrap()

def my_compute_function():
    print("My compute stuff")

time_compute(my_compute_function)$> My compute stuff
$> Time:  0:00:00
```

## 带参数的函数中的函数:

使用这种方法，您将无法将**参数**传递给“compute_function”。

让我们修改我们的代码:

```
import datetime as dt

def time_compute(compute_function):
    def wrap(*args, **kwargs):
        begin = dt.datetime.utcnow()
        compute_function(*args, **kwargs)
        end = dt.datetime.utcnow()
        print("Time: ", end - begin)
    return wrap

def my_compute_function(number):
    for i in range(number):
        print("{} my compute function".format(i))

time_compute(my_compute_function)(2)$> 0 my compute function
$> 1 my compute function
$> Time:  0:00:00
```

通过这些修改，我们可以将参数传递给需要计算耗时的函数。

*信息:*

> ***args** 是标准位置参数
> 
> ****kwargs** 是关键定义参数

您可以在此找到更多信息

# 装修工

在前面的例子中，我们已经看到，我们可以通过**包装**来修改函数**的行为**，以添加更多的逻辑。这就是**装修工**的主要目的。

```
time_compute(my_compute_function)(2)
```

## 简单装饰:

因为语法不容易阅读，所以“**@**”**操作符**出现了。因此，让我们将前面的例子转换成一个真正的 python 装饰器。

```
import datetime as dt

def time_compute(compute_function):
    def wrap(*args, **kwargs):
        begin = dt.datetime.utcnow()
        compute_function(*args, **kwargs)
        end = dt.datetime.utcnow()
        print("Time: ", end - begin)
    return wrap

@time_compute
def my_compute_function(number):
    for i in range(number):
        print("{} my compute function".format(i))

my_compute_function(2)
```

## 具有返回值的装饰器:

我们很容易地转换了简单的例子。但是我们遇到了另一个问题，如果我们的“my_compute_function”返回一个**值**，我们将永远得不到它，因为我们没有返回它。

我们来修改一下:

```
import datetime as dt

def time_compute(compute_function):
    def wrap(*args, **kwargs):
        begin = dt.datetime.utcnow()
        ret_value = compute_function(*args, **kwargs)
        end = dt.datetime.utcnow()
        print("Time: ", end - begin)
        return ret_value
    return wrap

@time_compute
def my_compute_function(number):
    value = 0
    for i in range(number):
        print("{} my compute function".format(i))
        value += i
    return value

value = my_compute_function(2)
print("Value: ", value)$> 0 my compute function
$> 1 my compute function
$> Time:  0:00:00
$> Value:  1
```

这里我们有一个完整的**装饰师**的例子。

## 多个装饰者:

我们有很多用例，你可以使用它们，比如日志、测试、监控等等。你也可以添加**多个装饰器**来增加你的功能。

```
import datetime as dt

def logg_function(compute_function):
    def wrap(*args, **kwargs):
        print("Call", str(compute_function), "with args:", [str(arg) for arg in args], "with kwargs", [key + '_' + str(arg) for key, arg in kwargs.items()])
        ret_value = compute_function(*args, **kwargs)
        return ret_value

    return wrap

def time_compute(compute_function):
    def wrap(*args, **kwargs):
        begin = dt.datetime.utcnow()
        ret_value = compute_function(*args, **kwargs)
        end = dt.datetime.utcnow()
        print("Time: ", end - begin)
        return ret_value

    return wrap

@logg_function
@time_compute
def my_compute_function(number):
    value = 0
    for i in range(number):
        print("{} my compute function".format(i))
        value += i
    return value

value = my_compute_function(2)
print("Value: ", value)$> Call <function time_compute.<locals>.wrap at 0x0000027BBDAB18B8> $> with args: ['2'] with kwargs []
$> 0 my compute function
$> 1 my compute function
$> Time:  0:00:00
$> Value:  1
```

如果你对更多的 python 技巧和窍门感兴趣，或者你想开发一个技术解决方案(网站、应用程序)，你可以在这里发消息联系我们[http://techsense-corporation.io/](http://techsense-corporation.io/)