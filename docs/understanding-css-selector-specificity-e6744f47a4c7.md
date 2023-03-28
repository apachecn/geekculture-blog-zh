# 了解 CSS 选择器特异性

> 原文：<https://medium.com/geekculture/understanding-css-selector-specificity-e6744f47a4c7?source=collection_archive---------17----------------------->

![](img/63a5193e8c1180a11265b825f9f73dbe.png)

CSS

我们都知道 CSS 有点神秘。我们都遇到过这样的情况，一个样式被应用，我们花了几个小时试图找出是哪个选择器在做这件事。当然，我们都退到了**！让我们的风格发挥作用的重要关键字**。当然，导致冲突的 CSS 仍然存在于代码库中，并将在我们的职业生涯或应用程序的生命周期中一直存在——无论哪一个先结束。希望我能在这篇文章中给你一些有用的信息，并澄清 CSS 带来的一些模糊之处。

我们将从应用 CSS 样式的顺序开始。我将按照应用 CSS 样式的顺序来列举和解释它们。

# 规则一:！重要样式

标有**的款式！重要的**将应用于任何其他非**！重要的**应用于它。
例如

```
.b**g-green {
  background-color: green !important;
}

#header {
  background-color: red;
}

<header clas**s=”bg-green” #header>
  Hi there, hey there, ho there!
</header>
```

在上面的例子中，背景色是绿色。标记为**的样式！重要信息**只能用另一个**覆盖**！重要的**样式来自选择器，但具有更高的特异性(参见规则 2)或具有相同的特异性，但在 CSS 文件中或在之后导入的 CSS 文件中。**

## 如何覆盖它

```
.bg-green {
  background-color: green !important;
}

#header {
  background-color: red !important;
}
```

在上面的例子中，背景颜色将是红色，因为 **#header** 选择器具有更高的特异性

```
.bg-green {
  background-color: green !important;
}

.bg-green {
  background-color: red !important;
}
```

在上面的例子中，背景颜色是红色的，因为背景颜色是红色的！重要信息两种情况下的样式具有相同的特性，但在样式文件中是最新的。

# 规则 2:选择器特异性

关于如何计算特异性，有各种风格/指南。我最喜欢也最容易理解的是版本号模式。在这种方法中，选择器将具有 A.B.C.D 模式形式的特异性，就像版本号是如何书写的一样。现在，让我们深入 4 个版本号中的每一个，解释如何计算它们。

## A.内嵌样式(1.0.0.0)

内联样式具有最高的特异性，将首先应用。它们只能被**覆盖！重要的**样式。这里 A 的值可以是 0 或 1，它要么设置了内联样式，要么没有。

```
header, 
#header,
header {
  background-color: red;
}<header id='header' style="background-color: green" class="header">
  Hey there!
</header>
```

## 如何覆盖它

```
<header id='header' style="background-color: green" class="header">
  Hey there
</header>header {
  background-color: red !important;
}
```

在上面的例子中，**背景色:红色！将应用重要的**样式— **参见规则#1**

## B.选择器中的 id 数(0。B.0.0)

在这里的版本号中，X 是选择器中 id 的数量。比如说。

```
#header #subtitle {
  background-color: blue;
}
```

在上面的例子中 B = 2，所以特异性数是 0.2.0.0。正如您可能猜到的，一个简单的 ID 选择器将具有 0.1.0.0 的特异性数字。这里有一个更复杂的选择器。

```
#header  ul>#home-link > .link-text:has(.active) {
  background-color: red;
}
```

在上面的选择器中，B = 2，因为我们在选择器中有 2 个 ID 选择器。

> 注意:ID 在整个 HTML 页面中是唯一的标识符，所以我强烈推荐使用上面例子中的**而不是**嵌套 ID 选择器，因为你可以直接选择 **#home-link** ，因为在文档中没有其他具有 home-link ID 的元素。您将它们链接起来的唯一原因是为了有目的地增加特殊性，以便覆盖一个样式，尽管这表明您可能需要解决一个不同的问题。

## C.类别/伪类别/属性的数量(0.0.C.0)

示例 1

```
.header > .link:hover {
  background-color: green;
}
```

上面的例子 C = 3，**。标题，。link 和:hover** 是给出这个数字的选择器。

示例 2

```
.header > .link[active] {
  background-color: green
}
```

本例中 C = 3，**。标题，。link 和【active】**是给出这个数字的选择器。请注意，属性选择器和类/伪类选择器对选择器的整体特性具有相同的影响。

示例 3

```
header > ul > .link {
  background-color: green
}
```

本例中 C = 1，**。因为我们只有一个类/伪类/属性选择器。**

## D.元素/伪选择器的数量(0.0.0.D)

示例 1

```
header > ul > li::after {
  content: "";
   background-color: red;
}
```

在上面的例子中 D = 4， **header，**后的 ul，li 和::是给出特异性的选择器。

示例 2

```
.header > ul > .link:active {
  background-color: red;
}
```

在上面的例子中，D = 1，因为只有 **ul** 是元素选择器。还有，C = 3，**。标题，。链接和:主动**给出该特异性。

## 几个组合的例子。

示例 1

```
#header  ul > li a:active {
  background-color: green;
}
```

特异性为 **0.1.1.2** ，1 个 ID 选择器，1 个伪类选择器，2 个元素选择器。

示例 2

```
.header #home-link::after {
  content: "";
  background-color: blue;
}
```

特异性为 **0.1.1.1** ，1 个 ID 选择器，1 个类选择器，1 个伪选择器。

示例 3

```
.header a {
  background-color: red;
}<header class=".header">
  <a style="background-color: blue">Homepage</a>
</header>
```

**背景色**样式的特异性是 **1.0.0.0** ，因为我们有内嵌样式。**的特异性。表头 a** 选择器为 **0.0.1.1** ，因此背景颜色为**蓝色**。

# 规则 3:排序顺序

这是非常直接的，给定两个或更多具有相同特性的选择器，最新声明的样式将是应用的样式。

```
.header {
  background-color: red;
}

/// other styles

.header {
  background-color: green
}
```

在上面的例子中，应用了**背景色:绿色**样式，因为它在样式文件中比**背景色:红色**更晚，并且两种样式具有相同的特性。

> 注意:如果我们在选择器中声明了具有不同特性的样式**，规则#2** 将决定应用哪个样式，如果有 2 个或更多具有相同特性的样式，则使用规则#3 。

# 规则 4:未声明的样式将被继承。

```
div {
  font-family: "Lato", sans-serif;
}<div>
  <p>This text will have Lato font</p>
</div>
```

在上面的例子中，p 标签还会应用**字体系列:“Lato”，无衬线**样式。如果我们没有为一个元素专门设置一个样式，CSS 会把它当作继承的。就像我们手动添加带有继承值的样式一样: **font-family: inherit** 在 **p** 元素上。

## 那么应用哪种风格呢？

当然是特异性更高的那个:)。在我们的版本号格式特性中，最新版本是被应用的版本。

下面举几个例子
1 . 0 . 0 . 0>0 . 9 . 9 . 9
0 . 5 . 2 . 1>0 . 5 . 2 . 0
0 . 9 . 3 . 2>0 . 9 . 2 . 9
0 . 9 . 0 . 0>0 . 8 . 9 . 9

## 一些提示

1.  尽可能降低选择器特异性。高特异性意味着覆盖样式需要更高的特异性。
    这里有一个关于坏/好选择器的例子。

```
/* good - 0.0.0.1*/
[type="number"] {
  font-size: 1.1em;
}

/* bad - 0.0.1.1*/
input[type="number"] {
  font-size: 1.1em;
}
```

2.尽可能避免覆盖样式

3.避免使用**！重要的**标志。节点那个**！重要信息**不管规则 2、3 和 4 如何，都会被应用，这意味着它使得样式非常难以跟踪

4.避免使用有多个 id 的选择器，因为 id 在一个文档中是唯一的，所以这些没有意义。

感谢阅读！一定要关注我更多的 Angular/TypeScript 和 CSS/SCSS 文章。