# 你懂 PHP 吗？

> 原文：<https://medium.com/geekculture/you-down-with-php-7ff93a9d8f5?source=collection_archive---------25----------------------->

## 第二部分

![](img/c95b128a028d6eb7d13045deb5a815f3.png)

Photo by [KOBU Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

所以在我的上一篇文章中，我介绍了一点 PHP 的历史。我们深入到变量、赋值操作符以及与。)点运算符。你可以在这里阅读那篇文章。在这一篇中，我们将继续学习语法。我们将讨论函数，并回顾 PHP 提供的一些开箱即用的内置函数。

## 功能

像所有其他语言一样，函数只是一组需要执行的指令。一个函数以关键字*开始函数。*接着是一个骆驼形状的名字*。*然后是一对包含参数*的括号。*后面跟着花括号*。*括号内是代码块*。*return 关键字将返回最终输出，从而退出函数。下面是一个函数的例子。

```
 function ultimateQuestion() {      return 42; }
```

要调用一个函数，你只需输入它，然后加上一对括号。

```
 function ultimateQuestion() { return 42; } ultimateQuestion(); //returns 42\. Which is the answer to the Ulitimate question about   
                 life, the Universe, and Everything
```

## 内置函数

在这一节中，我将介绍几个内置函数。我将给出语法和一个例子。

gettype 和 var_dump 都很相似，所以我将一起讨论它们。它们都接受一个变量并返回它的数据类型。主要的区别是 var_dump 提供了更多的信息。使用 var_dump，您可以获得数据类型、多少个字符以及变量包含的内容。下面是一些例子。

```
$name="Zaphod Beeblebrox";
$number=42;gettype($name);   //returns string
gettype($number); //returns integervar_dump($name);   //returns string(17) "Zaphod Beeblebrox"
var_dump($number); //returns int(42)
```

接下来，我们将讨论一些字符串函数。和大多数其他语言一样，PHP 有内置的函数可以改变字符串的大小写，还有一个函数可以改变单词的顺序，等等。先说这三个。

```
strrev ( string $string ) : string
echo strrev("Vogon");
   //Prints nogoVstrtolower ( string $string ) : string
echo strtolower("Vogon");
   //Prints vogonstrtoupper ( string $string ) : string
echo strtoupper("vogon");
   //Prints VOGON
```

对于最后一组，我们将讨论数学内置函数。一个返回整数的绝对值，另一个对数字进行舍入。

```
abs ( [mixed](https://www.php.net/manual/en/language.types.declarations.php#language.types.declarations.mixed) $number ) : int|floatabs(-42); //returns 42round ( float $val , int $precision = 0 , int $mode = PHP_ROUND_HALF_UP ) : floatrounds(41.5); //Prints 42rounds(42.2); //Prints 42
```

所以这就是 PHP 函数和内置函数的一点点内容。我将链接下面的 PHP 文档，这样你就可以看到许多其他可用的函数。

## 资源

[](https://www.php.net/docs.php) [## 证明文件

### PHP 手册在网上有多种语言版本。请从下面的列表中选择一种语言。更多…

www.php.net](https://www.php.net/docs.php)