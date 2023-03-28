# 使用节点。JS 文件系统

> 原文：<https://medium.com/geekculture/using-node-js-file-system-39e462c12c32?source=collection_archive---------12----------------------->

![](img/6dbbb72ff74041bc1219cbe202472249.png)

朋友们，你们好，这次我们将讨论 Node.JS 上的文件系统。

当我们开发应用程序时，我们总是要处理文件系统。文件系统，用于访问计算机上的文件。当我们在浏览器中运行 JavaScript 代码时，我们真的希望限制 JavaScript 访问文件系统。这种技术被称为沙盒。Sendboxing 将保护我们的程序免受恶意程序的攻击，以及窃取用户隐私的行为。

您可以在此了解更多教程:

[Javascript 中的日期方法](https://temanngoding.com/method-date-pada-javascript/)

[Javascript 错误处理](https://temanngoding.com/penanganan-eror-javascript/)

[Javascript 教程:将时间 am pm 转换为 24 小时](https://temanngoding.com/tutorial-javascript-convert-waktu-am-pm-to-24-jam/)

Node.js 提供了 fs 核心模块，可以让我们更容易的访问文件系统。fs 模块中的每个方法都有两个版本，即异步版本(默认)和同步版本。

要访问计算机上的文件，我们可以使用 fs.readFile()方法。该方法接受三个参数，即:文件位置、编码和一个回调函数，当文件访问成功/失败时将调用该函数。

# 读取 Nodejs 中的文件

在 Nodejs 中，有一个允许我们访问文件系统的文件系统(fs)模块。

文件系统模块通常用于:1

*   **读取文件；**
*   **写文件；**
*   **重命名文件；**
*   **删除文件；**
*   **等。**

这个模块已经在 Nodejs 中，所以我们不需要通过 **NPM(节点包管理器)**来安装。

让我们试着用 fs 模块读取文件。首先，请创建一个名为 index.html 的 HTML 文件，并填写如下:

我举几个例子。

访问。txt 文件使用文件系统。

索引. js

```
const fs = require('fs');

const fileReadCallback = (error, data) => {
    if(error) {
        console.log('Gagal membaca berkas');
        return;
    }
    console.log(data);
};
```

todo.txt

```
My Header
My paragraph.
```

并且还可以使用 fs.readFileSync()的同步版本。

基本上，使用 fs 的代码是一样的。唯一可以区分如何访问文件系统的方法。

我将给出一个使用 html 访问文件系统的例子。

索引. js

```
var http = require('http');
var fs = require('fs');
http.createServer(function (req, res) {
  fs.readFile('todo.html', function(err, data) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(data);
    return res.end();
  });
}).listen(8080);
```

todo.html

```
<html>
<body>
<h1>My Header</h1>
<p>My paragraph.</p>
</body>
</html>
```

请随意使用命令 node index.js 运行

# 创建 Nodejs 文件

除了读取文件，fs 模块还用于创建新文件。有几种方法可以用来创建文件:1

fs.appendFile()来创建和填充文件；
fs.open()创建、打开和写入文件；
fs.writeFile()创建并写入文件。让我们一个一个来试试。

首先，请创建一个名为 fs_append.js 的 javascript 文件，其内容如下:

```
var fs = require('fs');

//create a file named mynewfile1.txt:
fs.appendFile('mynewfile1.txt', 'Hello Teman Ngoding!', function (err) {
  if (err) throw err;
  console.log('Saved!');
});
```

# 使用 fs.open()函数创建文件

上面的程序代码将创建一个名为 mynewfile1.txt 的文件，内容为 Hello Friends Ngoding！。

创建一个名为 fs_open.js 的新 javascript 文件，内容如下:

```
var fs = require('fs');

fs.open('mynewfile2.txt', 'w', function (err, file) {
  if (err) throw err;
  console.log('Saved!');
});
```

fs.open()函数有三个参数:

1.  文件名；
2.  旗帜；
3.  打开文件时要执行的函数。

在上面的例子中，我们设置了 w (write)标志来告诉 fs.open()函数我们是否想要写或创建一个新文件。

如果没有指定名称的文件，fs.open()函数将创建一个空文件。

但是，如果文件已经存在，fs.open()函数将覆盖它。

然后当我们想只读文件时，我们可以给标志 r (read)。

除了 r 和 w，这里还有一些可以赋值的标志:3

*   r 打开文件进行阅读。如果文件不存在，将会出现错误。
*   r+打开文件进行读写。如果文件不存在，将会出现错误。
*   rs 以同步模式打开要读取的文件。
*   rs+以同步模式打开文件进行读写。
*   打开文件 e 进行写入。
*   wx 与 w 相同，但是如果文件已经存在，将会出错。
*   w+打开文件进行读写。
*   wx+与 w+相同，但是如果文件已经存在，将会出错。
*   打开文件进行填充。
*   ax 与 a 相同，但是如果文件已经存在，将会出错。
*   a+打开文件进行填充。
*   ax+与 a+相同，但是如果文件已经存在，将会出错。

# 用 Nodejs 重命名文件

在 fs 模块中有一个 rename()函数来改变名字。

该函数有三个参数:

1.  文件名；
2.  新名称；
3.  名称更改时要执行的功能。

创建一个名为 rename.js 的新 javascript 文件，并用以下代码填充它:

```
var fs = require('fs');

fs.rename('mynewfile1.txt', 'myrenamedfile.txt', function (err) {
    if (err) throw err;
    console.log('File Renamed!');
});
```

# 删除带有节点的文件

fs 模块有一个 unlink()函数来删除文件。该函数有两个参数:

要删除的文件的名称；
删除文件时要执行的功能。
创建一个名为 delete.js 的新文件，然后用以下代码填充它:

```
var fs = require('fs');

fs.unlink('mynewfile2.txt', function (err) {
    if (err) throw err;
    console.log('File deleted!');
});
```

因此，我传达这个教程，我希望它是有用的。

谢了。