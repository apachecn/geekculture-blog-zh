# 写一个程序把整数转换成罗马数字

> 原文：<https://medium.com/geekculture/write-a-program-to-convert-an-integer-to-a-roman-numeral-f25ae756d79d?source=collection_archive---------9----------------------->

为了解决这个问题，我们必须首先了解罗马数字的表示法及其原理。

罗马数字用 7 种不同的符号表示:

*   一(1)
*   五(5)
*   X (10)
*   L (50)
*   C (100)
*   D (500)
*   米(1000)

基于这些符号，我们有一些预定义的原则。例如:

1 - I
2 - II
3 - III

然而，4 没有被表示为 IIII
因为，我们用 V 来表示 5，从中减去 1 将得到 4，这将被定义为 IV。

同样的原则也适用于其他罗马数字。比如:`I`可以放在`V`和`X`之前。同样，`X`可以放在`L`和`C`之前，`C`可以放在`D`和`M`之前。

![](img/08e6283c95ec8d121c2577a319f701ba.png)

[Photo by Mike B](https://www.pexels.com/photo/clock-displaying-5-57-time-425027/)

现在我们知道了原理，我们将在 [JavaScript](https://www.javascript.com/) 中实现这个解决方案。

根据上述原则，我们将通过在数组中存储唯一的组合来开始我们的程序:

```
const values = [1000,900,500,400,100,90,50,40,10,9,5,4,1]
const romans =["M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"]
```

我们对程序的输入将是一个整数，我们将把这个输入与数组`values`进行比较，以确定该数字的范围，即 1 到 4、5 到 9、100 到 400 等等。

基于该范围，我们将开始将结果存储在字符串变量`result`中，并继续连接，直到我们得到最终结果(即，我们将使用`while`循环)。

```
let result = ""

for (let i = 0; num >0; i++) {
 while (num >= values[i]) {
    result += roman[i]
    num -= values[i]
 }
}
```

**例如:**

如果我们的输入是 445，那么它位于 400 到 500 之间，445 小于 500，那么分手会=> 445 = 400 + 45。

`result = CD + 45`

在 while 循环的下一次迭代中，我们将检查 45 的范围，即它位于 40 和 50 之间，因为 45 小于 50，所以拆分将是=>
45 = 40 + 5

`result = result + XL + V = CDXLV`

因此，我们的最终结果将是 CDXLV。

**完整解决方案:**

```
const values = [1000,900,500,400,100,90,50,40,10,9,5,4,1]
const romans =["M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"]const intToRoman = function(num) {
    for (let i = 0; num >0; i++) {
      while (num >= values[i]) {
        result += roman[i]
        num -= values[i]
     }
   }
};
```

## 结论

让我知道你是否喜欢这个或者你是否有其他解决这个问题的方法。

这是一个中等难度的编程问题，可以认为是技术面试的一个很好的练习问题。你可以在 [leetcode](https://leetcode.com/problems/integer-to-roman/description/) 上练习这个问题。

在我的下一篇文章中，我们将编写一个程序来创建一个罗马数字计算器。