# Nodejs 模块:路径模块。

> 原文：<https://medium.com/geekculture/nodejs-modules-in-five-minutes-the-path-module-f46445c8621d?source=collection_archive---------10----------------------->

![](img/df3918c96a34d78365be75a67b296439.png)

Node js 有一堆模块可以让我们作为开发人员的生活变得轻松，其中就有 path 模块。在我看来，path 模块是 nodejs 中最容易被误解的模块之一，今天我将尝试解释这是什么，path 模块附带的五个最常用的方法，以及如何在您的项目中实现它们。

## 节点中的路径模块是什么？

在节点 js docs 中，path 模块提供了使用文件和目录路径的实用程序。可以使用。是的，一个模块，将减轻你的生活时，与文件和目录。

像 Node 中的其他模块一样， **Path** 模块有很多方法。今天，我将讨论路径模块的五种方法。

# 1.Path.join()

在我看来，这种方法是路径模块中最常用的方法之一。初学者有时会混淆**解决**方法。

join 方法连接(顾名思义)所有传递的路径块并返回一个路径字符串。大概是这样的:

```
const path = require(‘path’)const dir = path.join(‘home’, ‘work’, ‘my-project’);console.log(dir);
//
home/work/my-project’
```

我知道你可能想知道为什么要使用 path 方法，因为你可以直接输入路径。join 方法不仅仅是连接路径段。它使用特定于平台的分隔符作为分隔符来连接这些段，然后对结果路径进行规范化。macOS 的分隔符与 windows 的分隔符不同。join 模块用特定于平台的分隔符连接这些路径段。

那很好。想象一下，你在一个由许多开发人员组成的团队中工作，其中一些人使用 Mac，而你使用的是 windows。显式指定路径将导致您的程序在团队成员的计算机上中断。

# 2.Path.resolve()

这是一个重要的路径方法，本文将是不完整的，如果错过了。此方法也与 join 方法相混淆。这两种方法都返回路径，但是 resolve 方法解析作为参数传递的路径段，并返回绝对 URL。

迷惑？让我们看看 resolve 方法将返回什么，传递我们上面的相同参数。

```
const path = require('path')

const dir = path.resolve('home', 'work', 'my-project');

console.log(dir);
//
'/home/siradji/projects/personal/community/dev.to/home/work/my-project'
```

# 3.Path.extname()

老实说，这些方法的名字是不言自明的。一看就知道这个方法和扩展名有关系。如果你这样认为，那你就对了。此方法返回给定文件的扩展名。让我们看一看:

```
const path = require(‘path’);const fileExtension = path.extname(‘/foo/bar/node.js’);
console.log(fileExtension);
//
‘.js’
```

请记住，此方法返回路径的扩展名，从最后一次出现。(句点)字符到路径最后部分的字符串末尾。

```
const path = require(‘path’);const fileExtension = path.extname(‘/foo/bar/node.js.md’);
console.log(fileExtension);
//
‘.md’
```

我最近用这种方法过滤掉了我编写的程序不支持的图像。一个用户上传了一个 png 文件，而我的程序(按照客户的指示)只想要一个 jpeg 文件。你可以看到我做那件事是多么容易。

# 4.Path.isAbsolute()

此方法将字符串路径作为参数，并返回一个布尔值。这是唯一返回布尔值的路径方法。这个方法用于检查一个给定的路径(作为参数传递)是否是一个绝对路径。

## 什么是绝对路径？

绝对路径总是包含根元素和定位文件所需的完整目录列表。如果要验证路径，可以使用此方法。

```
const path = require(‘path’);const isValidPath = path.isAbsolute(‘/foo/bar/node’);
// true
const path = require(‘path’);const fileExtension = path.extname(‘myfile.pdf’);
//false
```

# 5.Path.parse()

最后但同样重要的是，解析方法。这个方法，在我看来。非常酷。parse 方法接受路径作为参数，并返回一个带有路径“信息”的对象。“信息”以目录、扩展名、名称、基础、根的形式返回。当您想要提取有关路径的信息时，它非常有用。

```
const path = require(‘path’);const pathProps = path.parse(‘/foo/bar/node.js’);//
{
 root: ‘/’,
 dir: ‘/foo/bar’,
 base: ‘node.js’,
 ext: ‘.js’,
 name: ‘node’
}
```

就这样，伙计们！我希望你从这件事中学到了一些东西。我会写关于其他模块的后续文章，比如 Event、FS 和 OS 模块。

干杯，编码快乐！