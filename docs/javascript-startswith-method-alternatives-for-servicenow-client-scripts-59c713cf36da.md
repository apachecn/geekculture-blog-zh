# 检查 ServiceNow 客户端脚本中的字符串是否以特定字符开头

> 原文：<https://medium.com/geekculture/javascript-startswith-method-alternatives-for-servicenow-client-scripts-59c713cf36da?source=collection_archive---------7----------------------->

## ServiceNow 客户端脚本的 JavaScript 字符串 startWith()的两种替代方法。

![](img/b1d3033393fddf1a2491fcddd7cd75e3.png)

Photo by [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有时，当我们在 ServiceNow 客户端脚本中工作时，我们必须检查表单中以一组字符开头的字段。如果我们从事 web 开发，我们可以使用在[JavaScript ES6(ECMAScript 2015)](https://www.w3schools.com/js/js_es6.asp)中引入的 startWith()方法。一个简单易用的方法允许检查字符串的前缀，并根据给定的字符串是否以指定的字母或单词开头返回布尔值 true 或 false。在写这篇文章的时候，ServiceNow 不支持使用这种方法，所以一般来说，当我必须实现一个需要 startWith()这样的函数的需求时，我会使用这两种替代方法，这两种方法可以让我完成相同的输出，第一种是`indexOf()`，第二种是`substring().`

在讨论 indexOf()和 substring()之前，我们先来了解一下在现代 JavaScript 中我们是如何使用 startWith()的。

## startWith()语法:

`str.startsWith(searchString[, position])`

在这个例子中，我们使用`const,`声明了一个名为`str`的变量，关键字 const 也是 ECMAScript 2015。我们给字符串赋值 JavaScript 很棒’。我们可以使用浏览器控制台或者我最喜欢的 JS 片段测试器 [JSBin](https://jsbin.com/) 来测试它。

```
const str = ‘JavaScript is great’;
```

然后，我们用 console.log 登录浏览器调试控制台，用`startWith()`判断字符串是否以指定的字符串开头。如果字符串以指定的字符开始，这个方法返回`true`，否则返回`false`，记住这是区分大小写的。

在第一个例子中，预期的输出为真，因为指定的字符串与存储在`str.`中的值相匹配

```
console.log(str.startsWith(‘JavaScript is’)); //true
```

如果字符串不是以指定的字符开头，则返回 false。

```
console.log(str.startsWith(‘Java is’)); //false
```

第 4 行和第 5 行的其他错误退货示例:

```
const str = ‘JavaScript is great’; 
console.log(str.startsWith(‘JavaScript is’)); //true
console.log(str.startsWith(‘Java is’)); //false
console.log(str.startsWith(‘Python is’, 3)); //false
```

现在，如何在 ServiceNow 中使用 substring()实现上述操作。如果*简短描述*以单词“SAP”开头，我们希望向*简短描述*字段添加一条字段消息

## substring()语法:

```
*string*.substring(start_position [, end_position]);
```

start_position 是*字符串*中开始提取的位置。*字符串*中的第一个位置是 0，end_position 是 3，对于单词“SAP”的分隔如下/S/A/P，在“S”之前是零，如果不提供该参数，substring()方法将返回到*字符串*末尾的所有字符。

首先，让我们声明一个变量来分配一个简短描述的值。这是一个基于*简短描述的 *onChang* e 类型的客户端脚本。*

```
var shortDesc = g_form.getValue('short_description');
```

然后，我们使用条件`if`和`substring()`，我们为单词 SAP 指定字符串的开始和结束，在本例中为(0，3)。

```
if (shortDesc.substring(0, 3) == 'SAP') {
//do something if true
}
```

如果为真，则显示字段消息。

```
g_form.showFieldMsg('short_description', 'SAP Incident Ticket');
```

完整的客户端脚本:

```
g_form.showFieldMsg('short_description', 'SAP Incident Ticket');function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }
    var shortDesc = g_form.getValue('short_description');if (shortDesc.substring(0, 3) == 'SAP') {
        g_form.showFieldMsg('short_description', 'SAP Incident Ticket');
    }
}
```

现在，让我们使用 indexOf()。当我们操作一个字符串时，我们经常需要知道一个特定的元素是否存在，以及在哪个位置。当然，我们可以创建一个方法来遍历所有包含的元素，但是 indexOf()是为了节省我们的时间而特意构建的。

在浏览器控制台或 [JSbin](https://jsbin.com/) 中，我们尝试下面的脚本。

```
var str = "JavaScript is great!";
console.log(str.indexOf("JavaScript"));    // return 0
console.log(str.indexOf("Python"));// return -1
```

在上面的例子中，如果简短描述匹配，它将返回 0，如果不匹配，则返回-1，我们也可以将这个输出用于我们的客户端脚本。首先，我们声明我们的变量`shortDesc`，并为*简短描述*字段赋值。

```
var shortDesc = g_form.getValue(‘short_description’);
```

然后，我们使用一个 if 语句来检查条件是否为真，利用`indexOf().`

```
if ((shortDesc.indexOf(‘SAP’)) == 0) { //return true
 //do something
 }
```

对于这个特定的演示，我们在*简短描述*字段下方显示一条消息。

```
g_form.showFieldMsg(‘short_description’, ‘SAP Incident Ticket’);
```

使用 indexOf()检查简短描述是否以“SAP”开头的完整客户端脚本。

```
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }
    var shortDesc = g_form.getValue('short_description');
    if ((shortDesc.indexOf('SAP')) == 0) {
        g_form.showFieldMsg('short_description', 'SAP Incident Ticket');
    }
}
```

# 最终注释:

JavaScript 中有许多内置方法可以帮助我们处理字符串。这两个 substring()和 indexOf()可以作为 startWith()的替代。重述一下，substring()方法获取原字符串的一部分并作为新字符串返回，indexOf()方法起作用；不同的是，此方法返回指定值在字符串中第一次出现的位置，如果要搜索的值从未出现，则返回-1。