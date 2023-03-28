# 解决 Meta | Amazon | Google | Apple 面试问题。为 Array.flat()编写自定义实现

> 原文：<https://medium.com/geekculture/solve-meta-amazon-google-apple-interview-question-write-custom-implementation-for-array-flat-8e0072961c29?source=collection_archive---------2----------------------->

在一级和二级公司中，要求您用 JavaScript 实现内置方法的多填充/自定义实现是很常见的。由于以下原因，这些问题变得非常重要。

1.  **你对内置函数的了解**:除非你了解它，否则你不会为此编写 Polyfill。
2.  **编写聚合填充的基础知识:**这是一项重要的技能，它将决定你是否尝试过编写自己的自定义方法。

> 初学者:什么是 Polyfill

polyfill 是一段代码(通常是 Web 上的 JavaScript ),用于在不支持它的旧浏览器上提供现代功能。

你可以直接去下面的视频看我的解释。否则也可以看文章。

![](img/dcffc61f07a709aef681484edfd78cb2.png)

首先，我们需要了解什么是数组。平平？以及它是如何工作的？

> 数组.平面

`flat()`方法创建一个新的数组，所有子数组元素递归地连接到其中，直到指定的深度。

定义取自官方 Mozilla [文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)。

例如:输入:

```
const arr1 = [0, 1, 2, [3, 4]];console.log(arr1.flat());// output: [0, 1, 2, 3, 4]
```

> **语法**

```
flat()
flat(depth)
```

**Depth** :深度级别，指定嵌套数组结构应该展平的深度。默认为 1。

**在简单的字阵中。Flat 将嵌套数组展平为展平数组。可以使用参数指定深度。**

> 基本想法

要编写这个问题的自定义实现，首先我们需要了解这个问题的广义特征。用简单的方式，

1.  我们将遍历数组，数组索引是一个元素，将其推送到**结果**。如果没有，转到**步骤 2。**
2.  如果数组索引为数组，则:**转到步骤 1**

> 让我们学习这个问题的递归方法

在学习了递归方法之后，我们可以看到如何将它添加到定制实现中。

> **array . flat 的递归解**

```
let output = []
function flattenArray(arr){
        for (let i = 0; i < arr.length; i++) {
            if(Array.isArray(arr[i])){
                flattenArray(arr[i]);
            }else{
                output.push(arr[i]);
            }
        }
        return output;
 }
```

正如我上面解释的，我们正在遍历上面的数组，检查 Index 是否是数组，如果不是数组，我们就把值压入输出。如果它是一个数组，那么我们递归地迭代这个数组，并将值推入数组。

简单预演:

```
const output = []
const arr = [0, 1, 2, [3, 4]];**As long as array index is element:**
output = [0,1,2]**When array index is array.**
arr = [3,4]
output = [0,1,2,3,4]
```

> **为 Array.flat 编写自定义实现/polyfill**

```
Array.prototype.myFlat = function(){
    const output = []
    function flattenArray(arr){
        for (let i = 0; i < arr.length; i++) {
            if(Array.isArray(arr[i])){
                flattenArray(arr[i]);
            }else{
                output.push(arr[i]);
            }
        }
        return output;
    }
   const returnValue = flattenArray(this);
   return returnValue;
}const inputArray = [0, 1, 2, [3, 4, [5,6]], 7];console.log(inputArray.myFlat());
```

万一你不知道，如何添加方法到内置函数。请阅读我的文章[这里](https://mevasanth.medium.com/how-everything-is-object-in-javascript-a4164d7e6a2d)和[这里](https://mevasanth.medium.com/prototype-and-protypal-inheritance-in-javascript-bb766097ac05)。

读完上面的文章，你应该对原型继承有了公平的理解。那么上面的代码应该看起来容易理解。除了如何编写 Polyfills，上面对递归方法的解释也适用于此。

> **你的家庭作业**

尝试为上述问题编写迭代解决方案。万一你尝试了却没有解决，你可以在这里看我的视频[。](https://www.youtube.com/watch?v=0Mmx1ri70ME&list=PLmcRO0ZwQv4SNhbW4CI4vc-6IHBHCKzZN&index=4&t=2s)

感谢您在下一篇文章中阅读 catch you。如果你还没有在媒体上关注我，那么请关注我，你可以在链接的[这里](https://www.linkedin.com/in/vasanth-bhat-4180909b/)关注我。别忘了订阅我的 Youtube 频道[取消评论](https://www.youtube.com/channel/UCSCNvSCk_Z9mBvUM-FJexRg/videos)。

如果你想亲自和我讨论模拟面试，面试或简历审核的技巧和诀窍，你可以在这里预约:

[](https://topmate.io/vasanth_bhat) [## 在 topmate.io 上预订与瓦桑特的时间

### 为世界顶尖人才简化个性化互动

topmate.io](https://topmate.io/vasanth_bhat) 

你可以在这里观看我所有的定制实现/PolyFill 视频。

如果你正在准备前端开发者面试，请观看我的以下系列:

尝试为上述问题编写迭代解决方案。如果你尝试了但没有解决，你可以在这里看我的视频。

# 同一作者的更多文章:

1.  我在 JIO 信实公司的面试经历。
2.  [发出带有标题的 Get 请求，以在 React Native 中呈现图像](https://javascript.plainenglish.io/react-native-making-get-request-to-display-the-image-f75d4338c5e2)
3.  [JavaScript array . push()是深度拷贝还是浅度拷贝？](https://javascript.plainenglish.io/array-push-in-javascript-is-it-deep-or-shallow-copy-90cd195ec5b7)
4.  [如何在 JavaScript 中展平数组的数组](https://mevasanth.medium.com/flatten-array-of-array-in-javascript-microsoft-interview-question-345c71ff9ccd)

在这里阅读作者[的所有文章。](https://mevasanth.medium.com/)