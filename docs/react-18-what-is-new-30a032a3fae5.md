# 反应 18 —有什么新内容？

> 原文：<https://medium.com/geekculture/react-18-what-is-new-30a032a3fae5?source=collection_archive---------6----------------------->

![](img/f33c170f0aae7d193f936f27511a29fc.png)

*DOM(或文档对象模型)是网页内容的树状表示——由“节点”组成的树，这些节点根据它们在 HTML 文档中的排列方式而具有不同的关系。*

```
<div id="container">
  <div class="menuSection"></div>
  <div class="footerSection"></div>
</div>
```

在这里，`<div class="menuSection"></div>`是`<div id="container"></div>`的“孩子”，也是`<div class="footerSection"></div>`的兄弟。把它想象成一个家谱。`<div id="container"></div>`是一个**父**，它的**子**在下一层，每个子在它们自己的“分支”上。

> ***向 DOM 添加节点***

可以用 Javascript 将新节点添加到 DOM 中。createElement()方法创建一个新的元素节点。appendChild()方法向现有节点添加一个子节点。

```
let box = document.createElement("div");
box.style.color = "blue";
box.textContent = "new element";
document.body.appendChild(box);
```

这看起来不错，但是想象一下，如果我们有一个循环重复地向文档体添加节点。每次添加节点时，都会触发文档的重排，并影响应用程序的性能。

```
for(let i=0; i< 10; i++) {
  let list = document.createElement("li");
  list.style.color = "blue";
  list.textContent = "List Element" + i;
  document.body.appendChild(list);
}
```

以下主题有助于更有效地实现该场景。

> ***文档片段***

DocumentFragment 是一个没有父对象的文档对象。它用于追加节点。并且它不是活动文档树结构的一部分。因此，我们可以防止文档回流。

在将节点添加到实际的 DOM 之前，可以将 DocumentFragment 视为存储节点的临时盒子。

```
let fragment = document.createDocumentFragment();for(let i=0; i< 10; i++) {
  let list= document.createElement("li");
  list.style.color = "blue";
  list.textContent = "List Element" + i;
  fragment.appendChild(list);
}document.body.appendChild(fragment);
```

> ***目标节点带选择器***

当使用 DOM 时，使用“选择器”来定位您想要使用的节点。您可以使用 CSS 样式的选择器和关系属性的组合来定位您想要的节点。让我们从 CSS 样式的选择器开始。在下面的例子中

```
<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
```

你也可以使用关系选择器(如`firstElementChild`或`lastElementChild`等。)具有节点所拥有的特殊属性。

```
const container = document.querySelector('#container');
// select the #container div (don't worry about the syntax, we'll get there)

console.dir(container.firstElementChild);                      
// select the first child of #container => .display

const controls = document.querySelector('.controls');   
// select the .controls div

console.dir(controls.previousElementSibling);                  
// selects the prior sibling => .display
```

因此，我们根据某个节点与其周围节点的关系来识别该节点。

> ***DOM 方法***

## ***查询选择器***

*   *元素*。querySelector( *选择器*)返回对*选择器*的第一个匹配的引用
*   *元素*。querySelectorAll( *选择器*)返回一个“节点列表”,包含对*选择器*的所有匹配的引用

## 元素创建

*   document.createElement(标记名，[选项])创建标记类型 tagName 的新元素。`[options]`在这种情况下意味着你可以给函数添加一些可选参数。此时不要担心这些。

## 追加元素

*   *parentNode* 。appendChild( *childNode* )追加 *childNode* 作为 *parentNode* 的最后一个子节点
*   *parentNode* 。insertBefore( *newNode* ， *referenceNode* )在 *referenceNode* 之前将 *newNode* 插入 *parentNode*

## 移除元素

*   parentNode 。removeChild( *child* )从 DOM 上的 *parentNode* 中移除 *child* 并返回对 *child* 的引用

## 添加内嵌样式

*   首先，使用 DOM 方法选择元素，比如 querySelector( *选择器*)。所选元素具有 style 属性，该属性允许您为元素设置各种样式。
*   然后，设置样式对象的属性值。

感谢您阅读这个故事。

参考—[https://react js . org/blog/2021/06/08/the-plan-for-react-18 . html](https://reactjs.org/blog/2021/06/08/the-plan-for-react-18.html)

*原载于*[*https://codersread.com*](https://codersread.com/improve-performance-of-web-application-react-js/)