# JS:使用“Bind”的函数

> 原文：<https://medium.com/geekculture/js-function-currying-with-bind-4e3e210c41e4?source=collection_archive---------83----------------------->

![](img/863220a9b16ba385da91408e7d1c5576.png)

> [**Tutorialspoint**](https://www.tutorialspoint.com/) 将**curring**定义为一种将具有多个参数的函数求值为具有单个参数的函数序列的技术。

奉承是面试中最常被问到的问题之一。面试中经常遇到的一个问题是使用 currying 函数计算数字的总和:

```
sum(50)('*')............................// returns  50
sum(50)(40)('*')....................... // returns  90
sum(50)(40)(30)('*')................... // returns 120
sum(50)(40)(30)(20)('*')............... // returns 140Keep on adding the argument till `*` is encountered
```

像往常一样，我很快想到的方法是使用' **Closures '来解决这个问题。**我可以创建一个闭包来保存总值，直到收到**' ***。一旦 **'*'** 通过，返回总和。

因此，我的解决方案是:

```
**// I'm just using happy cases and no negative validations.**const sum = function(number) {
  let total = number;
  let curry = (arg) => {
      if (arg === '*') {
        return total
      };
      total = total + arg;
      return curry;
  }
  return curry;
}
```

该解决方案运行良好，并且完成了工作。然而，现在的想法是不使用闭包也能达到同样的效果。

# “捆绑”救援

我们可以在 bind 的帮助下实现同样的目标，而不需要任何终结。

***Bind 将第一个参数作为上下文，之后可以传递“n”个参数作为默认参数。***

因此，我们不是返回一个 curry 函数，而是返回父方法本身，将它绑定到一个**‘total’**值，这个值将给出结果。

```
**// I'm just using happy cases and no negative validations.**let sum = function (x) {
  let total = arguments[0];
  if(arguments.length === 1 && arguments[0] === '*'){
    return 0;
  }

  if(arguments.length === 2){
    if(arguments[1] === '*'){
      return arguments[0];
    }
    total = arguments[0] + arguments[1];    
  }

  return sum.bind({}, total);
}
```

这里，在每次调用时，一个新的方法被返回并绑定到一个参数，该参数保存所有先前调用的累积值。当遇到 **'*'** 时，返回实际值。