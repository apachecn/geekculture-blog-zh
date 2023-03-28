# 如何使用滚动动画制作动画

> 原文：<https://medium.com/geekculture/how-to-make-an-animation-using-animate-on-scroll-8f57ef73924c?source=collection_archive---------7----------------------->

## 一步一步的指导你如何使用滚动动画制作更好的网站设计(AOS)

![](img/5d1a2400e597edee8c2c7e39171fccec.png)

大约 3 个月前，在开发我的网站时，我想让我的网站更酷。这是我想做的:我希望我的网站上的项目，只有当我滚动到他们出现。换句话说，在滚动之前，该项目将不存在。难以想象？大致结果是这样的:

![](img/c7c03c7a0106eb6b8172da66a5de83f0.png)

这个动画可以由 michalsnik 在 [michalsnik.github.io](http://michalsnik.github.io) 用 Animate On Scroll (AOS)库创建。

# 开始

有两种方法可以将此库添加到您的网站。第一种方式是安装，第二种方式是通过 CDN 服务。我自己使用 CDN 服务。下面是您应该添加的一些代码。

**CSS**

将这段代码添加到 HTML 代码中的`head`标记内。

```
<link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet">
```

**Javascript**

将这段代码添加到您的`body`标签的最底部。

```
<script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
```

# 初始化 AOS

最后，将以下代码添加到您的 javascript 文件中。

```
AOS.init();
```

如果你已经做了以上所有的事情，那么你就可以使用这个库了。

# 动画

为了制作动画，将`data-aos`属性添加到你想要的 html 标签中。有了这个 data-aos，你的标签现在有了动画。`data-aos`属性有不同的值，不同的值意味着不同的动画。以下是其中的一些:

*   `data-aos="fade-up"`，用于上移
*   `data-aos="fade-down"`，向下移动
*   `data-aos="fade-right"`，向右移动
*   `data-aos="fade-left"`，为向左移动
*   `data-aos="fade-flip-left"`，用于向左翻转
*   `data-aos="fade-flip-right"`，向右翻转
*   `data-aos="zoom-in"`，用于放大
*   `data-aos="zoom-out"`，用于缩小

实际上，还有更多可用的数据 aos 值。更多完整的 data-aos，可以访问官方 Animate On Scroll 网站:michelsnik.github.io。

# 现场演示

我会告诉你如何使用 AOS 图书馆。

## 1.设置标准 html 标签

这个文件包含了基本的 html 标签来创建一个带有单词`box 1`、`box 2`等的框。

```
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>
<div class='container'>
<div class='box'>box 1</div>
<div class='box'>box 2</div>
<div class='box'>box 3</div>
<div class='box'>box 4</div>
</div>
</body>
</html>
```

## 2.为基本样式准备 CSS 文件

该文件包含`box`的基本样式，如背景色、填充、边距等。

```
.box{
  text-align:center;
  font-size:2em;
  padding:90px;
  border-radius:8px;
  border:5px #33ff33;
  margin: 30px;
  background:#66ff66;
  width:auto;
  height:30px;
}
.container{
  background:#3385ff;
  padding:auto;
  padding-top:20px;
  padding-bottom:20px;
}
```

## 3.添加 CDN 代码以导入 AOS 库

**CSS**

将这段代码添加到`head`标签中

```
<link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet">
```

**Javascript**

将这段代码添加到您的`body`标签的最底部。

```
<script src="https://unpkg.com/aos@2.3.1/dist/aos.js"></script>
```

## 4.初始化 AOS

将以下代码添加到 javascript 文件中。

```
AOS.init();
```

以上所有事情完成后，结果将是这样的:

请记住，您可以随意更改上述 CSS 的样式。在上面的例子中，我只使用了 4 种数据类型——AOS。您可以在该网站上查看更完整的文档:

 [## 美国歌剧协会(American Opera Society)

### 使用 CSS3 在卷轴库上制作 AOS 动画

michalsnik.github.io](https://michalsnik.github.io/aos/) 

要在网站上查看这些 AOS 数据的实现，您可以访问我的网站，它也使用下面的 AOS 库:

[](https://fikrinotes.netlify.app/javascriptproject-menu/boring_math/) [## 无聊的数学| fik ri mul yana setia wan 的数学解算器

### 枯燥的数学是解决数学问题的最佳方法。但我知道这很重要…

fikrinotes.netlify.app](https://fikrinotes.netlify.app/javascriptproject-menu/boring_math/) 

参考:

*   [https://michalsnik.github.io/aos/](https://michalsnik.github.io/aos/)