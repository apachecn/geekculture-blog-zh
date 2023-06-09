# 设计组件

> 原文：<https://medium.com/geekculture/designing-components-4af2e2cae4cc?source=collection_archive---------21----------------------->

![](img/ac6f3d61c870408dbb3432a4ba98090d.png)

我一直是组件的支持者。无论是 web 组件、聚合物组件还是框架特定组件(即 Angular、React、Vue 等。).我相信这是创建一个应用程序并为特定需求提供高水平的可重用性的最有效的方法。在这篇文章中，我想展示我在创建一个新组件时的思考过程。我并不想深入到组件的代码中，而是指出创建组件时应该考虑的事项。

在写这篇文章的时候，我一直在努力想出一个好的描述，同时使用“任何”组件类型的方法。我认为，如果我展示我如何处理一个特定的组件，会更容易理解。所以，现在开始…

# 要求

为了便于讨论，让我们假设我们需要一个管理电子邮件地址的组件。我们希望能够添加和删除电子邮件地址，每个都显示为一个药丸或一个标签，右边有一个小“x”删除地址。简单，切中要害，容易记忆。

我们最初的要求:

*   用于键入电子邮件地址的字段
*   按回车键、空格或逗号会将文本转换为字段下方的标签/药丸，清除该字段并确保该字段具有下一个地址的焦点
*   每个标签/药丸应该在文本的右边有一个“x”来删除电子邮件地址
*   单击标签/药丸应该允许您编辑电子邮件地址
*   如果时间允许，可能支持粘贴多个电子邮件地址

# 实施问题

现在，我们需要问自己一些关于这个组件将如何在应用程序中实现的问题。我们还需要考虑开发人员在实现这个组件时的期望。

问这些问题的最终目的是创建尽可能可重用的组件。当然，每个新功能都会增加时间，但从长远来看，最终可能会节省时间。这也可能会阻止创建做类似事情的多个组件。

1.  有人会试图用它作为反应性形式的对照吗？
2.  有人想附加某种验证器，显示不符合某个模式或其他标准的电子邮件地址吗？我们是应该在组件中包含验证，还是应该假设实现组件的任何人都会处理验证？
3.  我们如何告知用户存在无效的电子邮件地址？一个包含无效地址的容器，还是只是改变标签/药丸的样式？
4.  我们应该显示多少个电子邮件地址？当我们超过这个数字时会发生什么？如果我们最终隐藏了电子邮件地址，我们如何解决上面的问题 3？
5.  有人会想要设计它来满足他们的需求吗？或者设计应该是独立的，不允许改变？
6.  当事情发生时，有人可能需要或希望从组件中发出什么事件？
7.  出于某种原因，有人想在这个组件中添加自己的 UI 元素吗？也许是上面的地址 3？
8.  有人想用它来只显示有效/无效的电子邮件地址，而不能通过在字段中键入来添加新的电子邮件地址吗？
9.  这里有什么我没有想到的东西可以让它变得更加可重用吗？

# 回答我们的问题

以上问题的答案将影响我们如何开发这个组件。这些答案也会影响组件的可重用性以及实现的难易程度。我们的目标是让实现和匹配一些不同的用例变得非常容易。所以，我会回答上面的问题，并包括我对答案的看法。

1.  我想有人可能想用它作为反应型控制。然而，使该组件成为值访问器可能会妨碍其他一些实现用例。所以，让我们支持它，但也允许它的缺席。因此，我们需要添加一个属性，这样开发者就可以包含或不包含这个属性`formArrayControl`。
2.  我认为这是问题 1 的答案。反应形式允许容易地包含验证器。所以我认为如果我们包含一个反应式控件，验证器应该附加到这个控件上。否则，开发人员可以自己编写一个可以传递给组件的函数来处理这个问题。我们称这个属性为`validator`。然而，我们可能仍然希望包含一个基本的电子邮件验证器。
3.  这实际上是一个棘手的问题，因为有许多答案。我们可以在有效和无效的标签/药丸上包含一个样式类。我们可以将无效地址放入它们自己的容器中。我们可以只显示一条消息，类似于“有#个无效地址”。如果这只是为了显示的目的，那么`validator`属性可能不存在，这使得所有这些场景更加困难或不可能。
4.  为什么不让开发者来配置呢？然而，我们可能想要定义一个默认值。我们将添加另一个类似`emailDisplayCount`的属性。当超过该计数时，我们将只放置一个链接来显示所有这些。
5.  对于自定义样式，我认为使用带有默认值的 css 变量会很好。这确保了组件在开箱即用时就有很好的样式，但是如果需要的话可以定制。
6.  这东西应该发出的事件我觉得我很直截了当:`addEvt`、`deleteEvt`、`editEvt`、`invalidEvt`。也许是`loadEvt`和`destroyEvt`。
7.  我认为这个答案也有助于解决第三个问题。如果我们包括一个自定义 html 的插槽，比如显示无效的电子邮件地址。但是，如果有人希望在多个地方实现相同的实现，该怎么办呢？这意味着他们必须在每一个实现中为那个槽复制 html。因此，我会说，出于这个原因，我们不想支持这种情况。另外，我觉得这使得组件重用更加困难。然而，让我们继续通过添加另一个属性`showInvalidContainer`来支持无效地址容器。这实际上增加了第三个问题。
8.  我可以看到有人想要这个组件的显示部分。我认为支持这一点会使组件更加可重用。它还需要另一个我们称之为`displayOnly`的属性。
9.  为了查看我们是否遗漏了一些明显的东西，请与您的团队成员交谈，并听取他们对电子邮件地址管理器组件的意见。

# 发展结果/调整

在开发一个组件的过程中，有时你会得到一些想法，这些想法可能是最好的，或者是在需求收集过程中漏掉的。在这种情况下，如果时间允许，我通常会尝试将这些特性添加到组件中。

在开发这个组件时，我确实遇到了其他可能有用的用例。添加它们没什么大不了的，所以它们被添加了。我的结果可能有错误，但努力只是为了从这次讨论中得到一个结果。你可以在这里找到资源库:[https://github.com/keithstric/email-manager](https://github.com/keithstric/email-manager)。我这样做是作为一个有角度的组件，但这些思考过程适用于任何类型的框架、库或纯 web 组件。

*   我认为有一个防止重复的选项可能会很好。所以我们添加了另一个名为`preventDuplicates`的属性，如果这是真的，我们会阻止添加重复项并显示一条消息。我们还添加了另一个叫做`dupeEvt`的事件，即使`preventDuplicates`是`false`，它也会被触发。
*   我还为不同的容器添加了标签。`allContainerLabel`、`validContainerLabel`和`invalidContainerLabel`。
*   我最终没有实现多个电子邮件地址的粘贴。这一点被忽略了，主要是因为时间限制

# 结论

这篇文章实际上已经开始了，在过去的几个月里，当我试图把这些想法放在“任何”组件上时，我已经回顾了很多次。我发现这是最困难的，直到我确定最好的方法是只开发一个组件，并从这个角度来写这个过程。我认为这使得思考所有必要的步骤变得更加容易。

希望我已经向*展示了我在开发新组件时的*思维过程。我试着记住，我们希望一个特性的最健壮版本对开发者是可用的。即使添加的特性现在不是需求的一部分，因为它们可能很快会作为需求添加进来。

向组件添加额外功能的最大障碍是时间。对于这一点，我们真的不能做任何事情，除了得到我们能做的，也许把我们的思想、属性和方法留在组件中，只是不被使用。这样，当我们能够回到组件时，我们就有了一点领先优势。

直到下一次…编码快乐！