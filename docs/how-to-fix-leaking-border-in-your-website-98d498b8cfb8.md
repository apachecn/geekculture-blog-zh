# 如何修复网页设计中的漏边

> 原文：<https://medium.com/geekculture/how-to-fix-leaking-border-in-your-website-98d498b8cfb8?source=collection_archive---------13----------------------->

在你的 web 开发之旅中，你会遇到一些 CSS 问题，这只是时间问题，可能真的会令人沮丧。

我想避免你将来可能遇到的一些烦恼。我把这种有趣的行为称为“泄漏边界错误”

那么什么是泄露边界 bug 呢？

让我们假设你想练习你的 CSS 技能，并决定制作这样的东西:

![](img/aca27cca829e61a24d98edf79bb7ebbf.png)

One of many challenges in cssbattle.dev

我们有一个容器，然后 3 个圆绝对定位在该容器内。容器必须隐藏它的溢出，以免显示不必要的细节。

在这里，您可以看到 CSS 代码是如何实现的:

```
 body {
    background: #191919;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .main {
    background: #F2AD43;
    width: 150px;
    height: 200px;
    border-radius: 100px 100px 30px 30px;
    overflow: hidden;
    position: relative;
    border: none;
    border: 0px solid #191919;    
  }
  .left {
    top: 100px;
    left: -100px;
    position: absolute;
    width: 200px;
    height: 200px;
    background: #E08027;
    border-radius: 50%;
    z-index: 2;
  }
  .right {
    top: 100px;
    left: 50px;
    position: absolute;
    width: 200px;
    height: 200px;
    background: #824B20;
    z-index: 3;
    border-radius: 50%;
  }

  .sun {
    position: absolute;
    background: #FFF58F;
    width: 60px;
    height: 60px;
    border-radius: 100%;
    top: 40px;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 1;
    margin: auto;
  }
```

结果是这样的:

![](img/d5bb29458da281a9892a639ab50a2a53.png)

Same image as in top, but recreated with CSS

嘿，看起来一样！但是等等…让我们实际上更详细地检查它。右下角有什么问题…看起来背景实际上是溢出的？你可能会说背景是……漏出来的？

这个问题多见于 Chromium 浏览器。想想微软 Edge 或者谷歌 Chrome。在 Firefox 上，这个问题似乎不存在。

要修复这种奇怪的行为，请尝试在子元素上使用`background-clip: padding-box`属性。在我的情况下，那将是在`right`类。Mozilla 开发人员网络文档对此属性有如下描述。

> CSS 属性设置一个元素的背景是否延伸到它的边框、填充框或内容框下面。— MDN

现在，当修复应用时，我们可以尝试比较之前和之后。

![](img/94c3b86663545d1cd65d9dcc054c557d.png)

右图右下角的容器背景不再有泄漏。影响是轻微的，但有时非常明显。万一遇到这种问题，一定要试试这个解决方案。