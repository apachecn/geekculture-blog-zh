# 如何避免数据混乱？

> 原文：<https://medium.com/geekculture/how-to-avoid-data-chaos-f1d215371d27?source=collection_archive---------11----------------------->

![](img/0c898b40caf55f538be1ef987343c1d7.png)

Data Chaos (Pic Credit: aparavi.com)

即使你可能是以 Tableau 顾问的身份工作，你也不应该被这样的说法蒙蔽，即知道 Tableau 就足以成功。熟悉整个数据生态系统，这是成功的关键。

最近，我目睹了一场如此严重的数据混乱，以至于我整个五月都在疯狂地工作。最初，根据项目所有者的说法，项目的解决方案可以在后台处理(没有 Tableau ),这导致了严重的数据问题。但是，最终的解决方案是在前端提供的，即 Tableau。虽然我是一名 Tableau 开发人员，但对功能和后端系统的全面理解帮助我提供了一个可行的解决方案。

让我们来了解哪些知识可以帮助避免数据混乱，并通过提供适当的解决方案来帮助您提高工作效率。

## 理解功能系统

我所说的功能系统是指对客户业务运作的全面理解。尤其是项目所基于的部门职能或业务方面。很多时候，我们想当然地认为运营或系统的某些方面已经为所有相关方所知。我们假设客户端将提供必要的数据输出需求，我们只需应用如何在 Tableau 中实现它的逻辑。这里的关键词是“假设”。我们很容易忘记，就技术而言，客户也会做出很多假设。如果我们彻底了解客户的业务，我们可以避免许多不必要的混乱和问题。像这样，作为开发人员/分析师或顾问，我们也可以向客户提出战略、技术和逻辑建议。

## 了解 BI 数据生态系统

每个组织都有一个数据生态系统。视而不见会成倍增加我们的工作量。我们必须了解数据的来源、数据结构、数据类型、层次结构或细微差别，这些对您的项目有所帮助。这可以帮助你确定对你的项目至关重要的领域，尽管从客户的角度来看，这些领域可能被认为是不必要的。此外，如果有任何可以为项目增加价值的数据，您将知道在哪里搜索它们。

## 来自业务团队的需求文档

如果您从客户或业务方面请求文档，详细说明 BI 开发人员或顾问的要求、规范和期望，可以避免许多误解。文件不需要详细说明，但必须定义客户的期望。如果有任何困惑和/或疑问，本文件将成为参考点。这将让你和客户都清楚地了解你的期望。

## 提问

如果我们不问问题，我们会失去很多时间、精力和内心的平静。对于一个有影响力的交付品来说，提问是必不可少的。毕竟，它也影响你的绩效评估。提出问题，即使你觉得它们毫无意义或无关紧要。对客户来说，它们可能听起来无关紧要，但对你来说，在你的解决方案中应用逻辑可能是必不可少的。如果您在一开始就消除疑虑，并为每个相关人员节省大量时间，就可以避免数据混乱。主动要求提问时间。不仅要问系统和逻辑问题，还要问项目的可行性问题。

## 说不的能力

不要盲目同意所有的要求。技术不是解决任何或所有问题的灵丹妙药。过去有，将来也会有技术限制。依靠你的学识、技能、研究、过去的经验来得出项目需求可行性的结论。但是简单的拒绝是不够的。所以，准备提供一个替代的解决方案。这将使客户不那么防御性，并拓宽他们的视角，更开放的建设性对话。

## 在预定义的数据集上测试

避免在实时数据上测试您的工作，即使您正在模拟服务器上工作。请确保您的测试数据不会每小时或每天刷新一次。拥有一个预定义的静态数据集有助于评估您是否在计算字段或 SQL 查询中犯了任何错误，尤其是在使用 JOIN 语句时。像这样，你可以很容易地交叉评估结果。如果将来您因为新的需求而修改查询或解决方案，那么如果可能的话，使用相同的数据集来评估级联效应。您将能够立即识别代码或逻辑中的问题，如果有的话。

***总结*** ，希望这能让你更好地应对 BI 项目。但是，这就是需要知道的全部吗？可能不会，但这给了你一个大纲，让你准备如何明智、高效和有效地处理 BI 项目。直到下一次…