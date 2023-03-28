# JavaScript 中有用的不同种类的控制台输出

> 原文：<https://medium.com/geekculture/different-kinds-of-console-output-that-are-useful-in-javascript-1abfc971fdda?source=collection_archive---------1----------------------->

![](img/366e952a4f1088327418c24a282e9e89.png)

Different kinds of console output

嗨，我读到了 console.log，但是 console 还有其他非常有用的方法

# 清空控制台

是的，有一个方法可以清除控制台，如果你在 Chrome 浏览器中运行这个命令，你可以看到类似这样的东西

```
console.clear()
```

命令前

![](img/7f43ac016e717eaad3e9c83f649104e4.png)

example console.clear()

命令后

![](img/2fa359039e51d497e76791ad84941620.png)

Finish example console clear

# 数一数被调用了多少次

使用 console.count，我们可以计算一个变量字符串被调用的次数

```
console.count()
```

![](img/6840834ef3b88a5b61aedccdd86f3eac.png)

Code example of console.count()

![](img/75f8a0bd993a1da0cc7f6167d4cbca79.png)

Example of console.count()

# 检查对象的所有数据

显示对象的所有信息

```
console.dir()
```

![](img/d72ece1b5eb59b4bc7c044965e12ec83.png)

code of console.dir()

![](img/04bf30bb2eb476fbd6cd0f74fa724351.png)

example of console.dir()

# 在控制台上打印错误

使用 console.error()可以打印这样的错误

```
console.error()
```

![](img/5f80b8972a67d2957aec60aa07574a0a.png)

code of console.error()

![](img/fd24a173133f3630e9b680639d0964d3.png)

example of console.error()

# 用数据创建一个表

您可以控制数据输出并使用它们创建表格

```
console.table()
```

![](img/8307d541e97930a38edb4c5355c3b545.png)

code of console.table()

![](img/d664513b907d4ff841e3a58a86717f78.png)

Example of console.table()

# 查看时间

使用这种方法，我们可以检查响应的时间，或者检查人们检查某物或者点击某个区域的时间

```
console.time(), console.timeLog(), console.timeEnd()
```

![](img/9372185394e50a00898b6ebf4b38dbfe.png)

Code of console.time(), console.timeLog() and console.timeEnd()

![](img/699f41840a6c8a8f8064dfd329ee3b70.png)

Example of console.time(), console.timeLog(), console.timeEnd()

# 打印警告

用这种方法很容易产生警告

![](img/a72d407b554212406124adf0c9aed40c.png)

code of console.warn()

![](img/fa86aa39420399db4e09be3c4ac9e42f.png)

example of console.warn()

# 结论

还有其他一些方法，但这些对我来说是最有用的控制台方法，但在这种情况下，如果您想使用更多信息或错误或警告，可以使用 table，这些方法对 console.log 非常有用

# 来源

[https://developer.mozilla.org/en-US/docs/Web/API/console](https://developer.mozilla.org/en-US/docs/Web/API/console)