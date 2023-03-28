# async await 会阻塞 JavaScript 主线程吗？

> 原文：<https://medium.com/geekculture/does-async-await-block-javascript-main-thread-c07db9c48c3e?source=collection_archive---------3----------------------->

如果你已经登陆了这个页面，很可能你知道什么是 async 和 await，并且在某个时候你可能会有这样的疑问:他们真的阻塞了 JavaScript 主线程吗？

![](img/31670bf2b923de6546bcb7d9cdc87b49.png)

什么是异步和等待？

我相信你们大多数人都知道，对于那些不清楚的人，这里有一个简单的解释。

假设您想要使用 promise 进行 API 调用，该 API 调用的响应必须传递给另一个 promise，并且该 promise 的响应必须传递给另一个 promise。这被称为承诺链，如下面的代码所示。

```
function getUserData()
promise.then(response1 => {
 promise.then(response2 => {
  promise.then(response3 => {
     console.log("Inside the promise"); // Will be printed later
  }}
 }}
})getUserData()
```

上面的方法没有问题，事实上，如果你有一个承诺响应作为另一个承诺的参数传递，那么没有其他方法。大多数人都知道，在 JavaScript 中，promise 是用来实现异步任务的。所以在下面的代码片段中，“Before all promise”将被打印在“Inside the promise”之前。

```
function getUserData()
promise.then(response1 => {
 promise.then(response2 => {
  promise.then(response3 => {
     console.log("Inside the promise"); // Will be printed later
  }}
 }}
})getUserData()
console.log("Before all promise"); // will be printed first
```

在承诺执行之前，我们不会阻塞主线程的执行，一旦承诺完成执行，JavaScript 主线程就会从回调队列中挑选那些任务(如果你不知道 JavaScript 中的事件循环，可以在这个[链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)中找到它)。

**让我们进入异步话题，等待**

async 和 await 被认为是语法糖，而不是使用上面的承诺链，您可以使用 async、await 并编写非常结构化的代码，如下所示。

```
async function getUserData(){let response1 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');let response2 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');let response3 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');
}getUserData();
```

起初，如果你看着它，一切似乎都很好。但是如果您仔细观察，在执行第一个 await 和 response1 得到 API 调用的响应之前，控制不会传递给第二行，同样的情况会持续到 response3。你不认为它阻塞了主线程吗？

只是为了让你更困惑，看看下面的代码片段，并猜测输出。

```
async function getUserData(){let response1 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');let response2 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');let response3 = await 
fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');console.log("After all promise is executed");}getUserData();
console.log("Hello World");
```

将首先记录哪条语句？如果你在进一步阅读之前已经找到了答案，请在评论区添加你的名字。

答案是，“Hello World”将首先打印，在所有承诺执行后，将打印“所有承诺执行后”声明。

虽然这造成了混乱，但实际上 async 和 await 不会阻塞 JavaScript 主线程。如上所述，它们只是承诺链的语法糖。

换句话说，下面的两个代码片段是相同的。

片段 1:

```
function getUserData()
promise.then(response1 => {
 promise.then(response2 => {
  promise.then(response3 => {
     console.log("Inside the promise"); // Will be printed later
  }}
 }}
})getUserData()
```

片段 2:

```
async function getUserData(){let response1 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');let response2 = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');let response3 = await 
fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)');console.log("After all promise is executed");}getUserData();
```

由于函数开头有 async 关键字，JavaScript 会把这个方法执行从调用栈移到 web API，继续执行其他任务。这个话题很简单，但是在面试中，许多候选人看到上面的例子会感到困惑。这就是我写这篇文章来详细解释它的原因。

阅读愉快，在我的下一篇文章中再见。

**同一作者的更多文章:**

1.  [JavaScript 中的一切都是对象吗？](https://mevasanth.medium.com/how-everything-is-object-in-javascript-a4164d7e6a2d)
2.  [JavaScript 吊装:采访热点](https://mevasanth.medium.com/hoisting-in-javascript-hot-topic-for-interview-43b463a6a77?source=follow_footer---------0----------------------------)
3.  [JavaScript 中的记忆化——采访热点](https://mevasanth.medium.com/memoization-in-javascript-hot-topic-for-interview-815475544ab0)

在这里阅读作者[的所有文章。](https://mevasanth.medium.com/)