# JavaScript 中的 RequestAnimationFrame

> 原文：<https://medium.com/geekculture/requestanimationframe-in-javascript-82a913cf8c46?source=collection_archive---------0----------------------->

![](img/e568b54e6b9547c476f7be6844ecb21c.png)

使用本地的 requestAnimationFrame 方法，我们可以让我们的浏览器永远快速地重复某件事情。它调用自己来绘制下一帧。

📝注意:如果你想在下一次重画时制作另一帧的动画，你的回调函数必须再次调用 **requestAnimationFrame()** 。requestAnimationFrame()是 1 个镜头。

**requestAnimationFrame** 接受一个参数，只有回调。

**语法:**

```
window.requestAnimationFrame(callback);
```

**回调**:在下一次重画时更新动画时调用的函数。

带有 **requestAnimationFrame** 的动画是非阻塞的，这意味着如果您对 **requestAnimationFrame** 进行后续调用，所产生的动画将同时发生。

目标是**每秒 60 帧(fps)** 以出现平滑的动画。

所以我们确实喜欢这个👇

```
setInterval(() => {
  // animation code
}, 1000/60);
```

但是现在我们有了 **requestAnimationFrame** ，更加优秀和优化:

*   动画将如此顺利，因为它的优化
*   非活动标签中的动画将停止，让 CPU 冷却

让我们看看如何使用 requestAnimationFrame 创建上面的代码片段

```
function smoothAnimation() {
    // animtion
    requestAnimationFrame(smoothAnimation)
}
requestAnimationFrame(smoothAnimation)
```

# 你如何开始和停止动画⏲️

**requestAnimationFrame** 返回一个可以用来取消它的 ID。

```
let reqAnimationId;
function smoothAnimation() {
    // animtion
    reqAnimationId = requestAnimationFrame(smoothAnimation)
}// to start
function start() {
    reqAnimationId = requestAnimationFrame(smoothAnimation)
}// to end
function end() {
     cancelAnimationFrame(reqAnimationId)
}
console.log("reqAnimationId", reqAnimationId)
```

点击**打开**查看更多详情:

# 参考🧐

*   [request animation frame 的 MDN 文档](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)
*   [保罗·爱尔兰请求动画帧](https://www.paulirish.com/2011/requestanimationframe-for-smart-animating/)

# 总结#

如果您在浏览器上制作任何动画，并希望对其进行优化，那么我们强烈建议您使用 requestAnimationFrame web API。

感谢你阅读❤️的文章，希望你喜欢这篇文章。

🌟[推特](https://twitter.com/suprabhasupi)👩🏻‍💻suprabha.me🌟 [Instagram](https://www.instagram.com/suprabhasupi/)