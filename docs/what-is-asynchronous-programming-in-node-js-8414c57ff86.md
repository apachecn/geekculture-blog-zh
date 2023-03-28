# Node.js 中的异步编程是什么！？🤔🧐

> 原文：<https://medium.com/geekculture/what-is-asynchronous-programming-in-node-js-8414c57ff86?source=collection_archive---------7----------------------->

![](img/80869f76ea586b9be5366ca77f6e5d11.png)

异步 I/O 是一种输入/输出处理形式，它允许在传输完成之前继续进行其他处理。

JavaScript 中的异步编程只有在函数是该语言的一等公民的情况下才能实现。因此，函数可以像其他变量一样传递给其他函数。

> *能以其他函数为自变量的函数称为高阶函数。*

在 Javascript 中，处理异步或昂贵操作的一种有效方式是使用承诺。这是因为在异步操作中，无法保证操作将在何时完成以及它将返回的结果。因此，promise 对象帮助我们适应操作的中间状态，无论是成功(解决)、挂起(正在进行)还是失败(拒绝)。

```
Promise.resolve() //success
Promise.reject() // fail
```

# 为什么承诺？

我们在上面建立了 Promise 对象代表异步函数的结果。承诺可以用来避免回调的连锁，称为`Callback hell`。

# 让我们从将节点样式的回调转换为 promise 开始。

## 假设，让我们为讨论定下基调😃

*   你熟悉 javascript 吗
*   并且您对节点环境有一些经验；如果这些都检查过了，✅，我们就可以开始学习承诺了😇。

在处理异步操作时，使用承诺更容易，但不幸的是，Nodejs 的许多 API 都是用回调编写的。(承诺是对 ECMAScript 的更新)

下面是一个使用文件流 API 的示例。`fs.readFile`

```
const fs = require('fs')fs.readFile(filePath, options, callback)
```

这里，回调函数至少包含两个参数。第一个参数必须是错误对象和返回的数据。

```
fs.readFile('file-path', (err, data) => {
  if (err) {
     //throw new error
  } 
    //do something with data return})
```

下面我们使用`util.promisify` API 将回调样式的节点 API 转换为承诺

```
const util = require('util');
const fs = require('fs');const readFilePromise = util.promisify(fs.readFile)
readFilePromise('file-path').then((data) => {
    //do something with data return
}).catch((error) => {
    //throw new error
});
```

我们还可以使用`async/await`操作来解决一个承诺。

```
const readFilePromise = util.promisify(fs.readFile) async function(){ 
   const data = await readFilePromise; 
   console.log(data); }
```

# 让我们创造承诺

在 javascript 中，我们可以创建返回承诺的异步函数😊。请参见下面的示例。

```
const myPromise = new Promise((resolve, reject) = { setTimeout(() => { 
       console.log('resolving the promise ...');               resolve('resolved!'); }, 5000); reject('rejected!'); }); console.log(myPromise);
```

我们可以使用`.then()`和`.catch()`高阶函数来链接承诺。`.then()`接受回调作为参数，并调用解析后的承诺。`.catch()`接受回调，并在承诺被拒绝时执行。

请参见下面的示例。

```
const resolvedCallback = console.log(data); 
const rejectedCallback = console.log(err); myPromise.then(resolvedCallback).catch(rejectedCallback);
```

# 处理多个独立承诺的更多示例

与我们上面写的例子不同，我们可以无条件地返回一个 promise 对象的状态。当我们需要`resolve`或`reject`多个独立的异步操作时，这可能是有用的。就像写字一样简单；

```
Promise.resolve();
Promise.reject();
```

我们注意到在节点环境中，Promise 对象是全局可用的，有趣吧！😎

当处理多个独立的承诺时，方法`Promise.all()`和`Promise.race()`会很有用。这两种方法都接受一组承诺作为参数。

使用`Promise. all()`时，只有在最后一个承诺为`resolved`后，才会调用`.then`方法。

```
const promiseArr = [
  Promise.resolve("data1")
  Promise.resolve("data2")
  Promise.resolve("data3")
]Promise.all(promiseArr).then(results =>
  console.log(results)
)// [ data1, data2, data3 ]
```

使用`Promise.race()`时，只有当第一个承诺为`resolved`时，才会调用`.then`方法。

```
Promise.race(promiseArr).then(results =>
  console.log(results)
)// data1
```

还需要注意的一点是，开销最小的函数总是首先解决。这使我们能够运行多个异步操作，并对最先完成的操作进行操作。

# 总结✍️

*   承诺代表功能的执行状态
*   承诺有三种状态；解决、拒绝和待定。
*   使用 async 和 await 是包裹在承诺周围的糖衣，它使处理承诺变得容易和可读
*   Await 确保它解决正在执行的承诺，并鼓励尽早返回错误。
*   承诺和异步/等待完成同样的事情。
*   它们使得检索和处理昂贵的函数/异步数据变得更加容易。
*   它们消除了回调地狱/嵌套函数的需要
*   简化错误处理。
*   减少无关代码
*   让等待多个并发调用返回和在调用之间添加额外代码变得更加容易。

感谢观众，我希望这篇文章对你开始使用 Javascript 的承诺有所帮助🤗。欢迎在 [Github](https://github.com/nextwebb) 、 [Twitter](https://twitter.com/i_am_nextwebb) 和 [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) 上联系我。一定要点赞、评论和分享😌。

[了解更多关于优雅异步编程的承诺](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises)

*最初发布于*[*https://blog . next Webb . tech*](https://blog.nextwebb.tech/asynchronous-programming-in-nodejs)*。*