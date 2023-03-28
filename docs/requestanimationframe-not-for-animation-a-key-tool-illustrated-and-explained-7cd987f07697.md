# requestAnimationFrame()不用于动画——一个关键的工具

> 原文：<https://medium.com/geekculture/requestanimationframe-not-for-animation-a-key-tool-illustrated-and-explained-7cd987f07697?source=collection_archive---------8----------------------->

如果你是专家，测试你的理解能力

![](img/1419e1c01281504465ea597370b84ade.png)

The nice image is borrowed from [http://clipart-library.com/clipart/453662.htm](http://clipart-library.com/clipart/453662.htm)

`requestAnimationFrame()`用于指示浏览器在重画 DOM 之前立即执行回调。该方法确保通过其回调引入到 DOM 中的任何更改都显示在下一帧中。