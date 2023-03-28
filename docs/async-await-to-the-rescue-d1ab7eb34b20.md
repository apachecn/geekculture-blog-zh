# 异步等待救援

> 原文：<https://medium.com/geekculture/async-await-to-the-rescue-d1ab7eb34b20?source=collection_archive---------20----------------------->

![](img/ea5ac66f08acc3f5d5b97b4178ce4f12.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

以前，我很少在我的 JavaScript 代码中使用 *async await* ，一开始我总是使用 *fetch* 方法来获取我需要的任何数据。随着时间的推移，我开始重新审视它，结果发现实现它并不难。

在这里，我想尽可能简单地解释一下*异步等待*。试着看看这段代码。

```
function usingFetch() { fetch('https://randomuser.me/api/') .then(parse => parse.json()) .then(result => {
            // do something with the result
        }}
```

上面的代码是通常的获取方法，这看起来不太好，因为所有关于我们刚刚得到的数据的操作都需要在第二个*中进行。然后，*或者将该值传递给另一个函数或变量。

试着看看下面的代码进行比较。

```
async function useAsync() { let getData = await fetch('https://randomuser.me/api/'); let result = await getData.json(); // do something with result}
```

在第 2 行中，在 *fetch* 之前的 await 关键字意味着我们需要等到获取过程完成，这样我们就可以在那之后执行下一行。同样的过程发生在第 3 行，我们需要等待它完成数据解析后再执行下一行。

通过使用 *async await，*代码可以变得简单、干净，并且可读性更好。需要注意的是，我们需要在函数初始化之前放置一个异步字，这样我们就可以在函数中使用 *await* 关键字。需要记住的是，如果函数初始化中没有 *async* 关键字，那么 *await* 关键字将不起作用。

# 结论

只要试一试，一旦你习惯了，它就会变得非常方便，并避免我们从*回调地狱*。希望大家喜欢，谢谢！