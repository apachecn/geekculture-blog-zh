# 如何不用 JavaScript 渲染“Hello World”

> 原文：<https://medium.com/geekculture/how-not-to-render-hello-world-in-javascript-7d3ce7c36956?source=collection_archive---------28----------------------->

![](img/37732b8ce4516bb5abb7ab432b61025e.png)

Photo by [R Mo](https://unsplash.com/@mooo3721?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

他们说，至少我们最大的优势就是我们最大的弱点。在某种程度上，JavaScript 也是如此。由于我们可以用这种编程语言表达自己的方式很多，我们很容易被过多的可能性淹没。

甚至一个很基本的输出*你好，世界！*可以用多行代码进行渲染。在这里，我们将仔细研究 4 个 JavaScript 函数，以便更好地理解什么时候不使用它们更明智。

四大巨头分别是`innerHTML`、`insertAdjacentHTML`、`textContent`和`innerText`。

1.  你好世界！与 **innerHTML**

属性 innerHTML 为我们提供了将字符串值作为 HTML 插入代码的可能性。它也可用于插入纯文本，如下图所示(*1–1*和*1–2*)。

*Figure 1–1*. HTML — a <p> tag with a ‘target’ id.

*Figure 1–2*. JavaScript— innerHTML property used to set text of target element.

事实上，上面的代码帮助我们输出想要的信息。尽管如此，使用这种属性还是有一些主要的缺点。

首先，它容易受到安全威胁的攻击，比如 JavaScript 注入。这反过来会导致许多不良后果，如信息泄露、修改设计和其他内容。

innerHTML 的另一个令人担忧的方面是它的速度，这一点在某些情况下比在其他情况下更明显。如案例 B(figure 1–2)所示，我们可以使用这个属性将 HTML 添加到目标中已经存在的元素中。这里的问题是，在我们的浏览器中这是相当低效的。这是因为只有在我们的目标元素的所有子元素都被处理之后，浏览器才会添加我们的信息。这需要时间。目标中的子元素越多，花费的时间就越多。

**什么时候不使用 innerHTML 更明智？**

*   当我们想要减轻恶意 JavaScript 注入的风险时。
*   当我们想快速追加 HTML 时。
*   当我们想要插入纯文本时。

2.你好世界！使用 **insertAdjacentHTML**

属性 insertAdjacentHTML 也为我们提供了将字符串值作为 HTML 插入代码的可能性。它允许我们选择放置内容的位置。这是在我们的目标元素之内或之外，正如你在下图(2-1)中看到的。

Figure 2–1\. JavaScript - insertAdjacentHTML property used to append text to the target element.

这一特性不仅在方便方面是有益的。它也比 innerHTML 快得多。事实上，insertAdjacentHTML [在 5 秒的时间内成功地附加了比 innerHTML](https://hacks.mozilla.org/2011/11/insertadjacenthtml-enables-faster-html-snippet-injection/) 多 150 倍的推文(30 000 条推文对 200 条推文)。考虑到浏览器在 insertAdjacentHTML 的情况下不会像处理 innerHTML 属性那样以低效的方式处理我们的目标元素的子元素，这些结果应该不会令人惊讶。

因为这种方法将传递的内容解释为 HTML，所以它会遇到与 innerHTML 相同的安全风险。

**什么时候不使用 insertAdjacentHTML 更明智？**

*   当我们想要插入纯文本时。
*   当 HTML 源不可信时(与安全的后端相比)。

3.你好世界！与 **innerText**

属性 innerText 以纯文本形式插入传递的信息，这与前面讨论的 innerHTML 和 insertAdjacentHTML 函数不同。这使它成为显示用户输入的更安全的替代方法。

Figure 3–1\. JavaScript — innerText property used to append text to the target element.

然而，innerText 不是一个防弹属性。它的计算量很大，会触发 DOM 回流。重新计算文档中现有元素信息的过程。不足为奇的是，这需要时间，并且具有降低性能的非常不希望的效果。最重要的是，旧的 Firefox 版本(2–44)不太支持这个属性。

什么时候不使用 innerText 更明智？

*   当我们想要插入文本而不牺牲速度和性能时。
*   当我们想在旧版本的 Firefox 中插入文本时。

4.你好世界！与**文本内容**

另一种写法 *Hello World！*在页面上是借助 textContent 属性，如下图所示。

Figure 4–1\. JavaScript — textContent property used to append text to the target element.

与上面讨论的其他 JavaScript 函数相比，这个属性提供了很多好处。

首先，它不会像 innerHTML 和 insertAdjacentHTML 那样带来安全风险。第二，它不像 innerText 那样计算量大，因为它不会导致回流。第三，旧版本的 Firefox 和 Chrome 很好地支持它。

然而，没有一种财产是完美的。textContent 也是如此。因为旧的 Internet Explorer 版本(8 及更低版本)不支持它。此外，返回值也有细微差别。

*Figure 4–2*. HTML — a <p> tag with a ‘target’ id and a hidden span element.

Figure 4–3\. JavaScript — differences of return value between innerText and textContent.

正如我们在上面的图(4-2)中看到的，一个隐藏的 span 元素包含文本*隐藏。*由 textContent 而不是 innerText 属性返回。这说明 textContent 也返回隐藏元素的文本内容，而这在某些情况下是不可取的。

**什么时候不使用 textContent 更明智？**

*   当我们想在旧版本的 Internet Explorer 中插入文本时。
*   当我们不想返回隐藏的元素时。

总之，上面讨论的所有属性都可以在适当的上下文中成功使用。然而为了渲染 *Hello World！textContent 的缺点最少，通常是最明智的选择。*