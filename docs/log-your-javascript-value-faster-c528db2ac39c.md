# 更快调试 JavaScript 的技巧

> 原文：<https://medium.com/geekculture/log-your-javascript-value-faster-c528db2ac39c?source=collection_archive---------7----------------------->

![](img/cabdb04f1a1e7008851313c7c3c7c50a.png)

Photo by [Stephen Dawson](https://unsplash.com/@dawson2406?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果您是一名开发人员，在编写相同的 *console.log()* 语法来检查当前变量值之后，您可能会发现自己很累。这里有一些更快记录你的价值的方法。

## 创建日志功能

您可以在 JavaScript 文件上创建这个函数，或者如果您想在多个文件中使用它，也可以在单独的文件中创建它。这个函数非常简单，就像这样。

```
function log(...params){
    console.log(...params)
}
```

注意，我们在函数参数和 *console.log* 语句中都使用了 spread 运算符，因为我们想要迭代传递给参数的所有项目，并将其打印到 *console.log* 语句中。

在 JavaScript 文件下面的某个地方，你可以像普通的 *console.log* 语句一样使用这个函数。

```
let yourVariable = 'Print me!';log(yourVariable)
```

或者类似于，

```
log('this is the data: ', yourVariable)
```

## 创建代码片段

如果您使用 vscode 作为代码编辑器，可以单击左下角的齿轮设置，然后选择用户代码段。您可以选择现有的代码片段或创建新的代码片段。您可以像下面这样编写代码片段。

```
"log": {
   "prefix": "getlog",
   "body": "console.log()"
}
```

log *键*只是代码片段的名称，但是*前缀*值是您将在编辑器中键入以获得正文值的单词。因此，如果我在我的 vscode 中键入 *getlog* ，一个 *console.log* 建议会在我键入时出现。

就是这样！感谢您的阅读，再见！🍻