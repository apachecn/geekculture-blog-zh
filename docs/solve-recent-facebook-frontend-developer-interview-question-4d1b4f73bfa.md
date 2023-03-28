# 解决最近脸书前端开发者面试问题

> 原文：<https://medium.com/geekculture/solve-recent-facebook-frontend-developer-interview-question-4d1b4f73bfa?source=collection_archive---------2----------------------->

在我在网上做了很多研究后，我得出一个结论，没有专门为前端开发者准备的教程/视频系列。所以，我决定在我的 [Youtube 频道](https://www.youtube.com/results?search_query=uncommon+geeks)中解码 MAANG 最常见的面试问题。在这篇文章中我将讨论一个非常有趣的问题。所以，一直读到最后。

![](img/0ea215d5ababd7beab9507f0b96a3fb3.png)

Facebook interview question solved

> **问题:**清除屏幕上出现的所有超时。

问这个问题的公司**:脸书/Meta**

**你通常的思考过程:**使用 **clearAllTimeout** 方法，清除超时。

**方法的问题:**JavaScript 中没有名为 **clearAllTimeout** 的函数。只有 **clearTimeout** 功能。

**关于超时的一些基本信息**

**setTimeout** 函数用于实现 JS 中的异步任务。setTimeout 的返回值是整数。这个值可以传递给 **clearTimeout** 方法来清除超时。

ex:const x =**setTimeout(()=>{ }，1000)；// x** 可以是任何数字**例如:1**

**clearTimeout(x)** //这将清除上面创建的超时。

你可以在官方文档中读到更多关于 setTimeout 的信息。

> **为什么这个问题很重要**

当用户从**第一页**导航到**第二页**并且在用户导航到**第二页**后在第一页上创建的超时，那么**第一页的**超时将占用内存来执行，这是无意的。

> **解决方案:**我们需要以某种方式跟踪超时，并最终清除它们。
> 
> **解决方案 1:** 全局变量方法。

保存一个全局数组，并将所有超时返回值放入该数组，当用户访问该页面时，清除所有超时值。

```
var timeoutArr = [];const timer1 =  setTimeout(() => {console.log('Time 1')}, 1000)
timeoutArr.push(timer1);const timer2  = setTimeout(() => {console.log('Time 2')}, 1000)
timeoutArr.push(timer2);timeoutArr.forEach(item => clearTimeout(item))
```

**这可能是第 2/3 层公司可以接受的解决方案。如果你渴望进入像脸书/梅塔这样的公司，那么你需要写一份略微理想的方法。**

> **解决方案 2 :** 实用文件方法。

与其使用全局数组，不如在实用程序文件中使用数组。

**工作代码**

```
export const TIMER_UTIL = {
    timerArr: [],
    setTimeout: function(fn, delay){
        const id = setTimeout(fn, delay);
        this.timerArr.push(id);
    },
    clearTimeout: function(){
        while(this.timerArr.length){
            clearTimeout(this.timerArr.pop())
        }
    }
}TIMER_UTIL.setTimeout(() => console.log('First timer'), 1000)
TIMER_UTIL.setTimeout(() => console.log('Second timer'), 1000)
TIMER_UTIL.clearTimeout();
```

我们可以跨不同的文件导入 **TIMER_UTIL** 并调用 **TIMER_UTIL.setTimeout** ，这将创建一个超时并将值推入一个数组。

当你活在页面中时，你可以调用**TIMER _ util . clear time out()；**这将清除到目前为止创建的所有超时。

感谢您阅读下一篇文章。如果你没有在媒体上跟随我，请跟随。别忘了订阅我的 Youtube [频道](https://www.youtube.com/channel/UCSCNvSCk_Z9mBvUM-FJexRg/videos)，

如果你想亲自和我讨论模拟面试，面试或简历审核的技巧和诀窍，你可以在这里预约:

[https://topmate.io/vasanth_bhat](https://topmate.io/vasanth_bhat)

如果你正在准备前端开发人员面试，请观看我的以下系列:

如果你想学习内置方法的 JavaScript 自定义实现，那么看下面我的系列: