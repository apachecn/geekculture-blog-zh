# 借助 Power BI 进行数据分析

> 原文：<https://medium.com/geekculture/data-analytics-with-power-bi-89a208b7c126?source=collection_archive---------39----------------------->

我用我的机器学习知识研究数据科学技术已经有一段时间了。在使用数据科学时，数据分析对于理解数据非常重要。我想到给阿碧权力去进一步探索。这篇文章将仅仅包含一些关于如何做不同的数据分析的概念性工作。开始吧！

"D *数据分析是一种技术层面的分析，具有预测能力，可用于寻找有效的业务解决方案！现在，Power BI 是一种先进的分析工具，可以减少手动工作，并能够帮助组织做出更好的业务决策，创造可操作和有意义的结果。”*

在本文中，我将展示如何使用 PowerBI 来完成数据分析任务。数据分析包括五个阶段:1 .准备数据 2。数据建模 3。可视化数据 4。分析数据。发布和维护交货。在使用 PowerBI 完成这些阶段时，我将剖析一些非常直观的技术。首先，我们需要一些数据来连接 PowerBI。先说第一阶段！

## 准备数据:

我将使用库存、地理、营销、调查和竞争情报数据。数据集可以在[***Google Drive***](https://drive.google.com/drive/folders/1FYS1rE7EAIknT7iJoba6rPK--AA5GJHT?usp=sharing)这里找到。

要将 CSV 文件从本地计算机加载到 PowerBI:

> 获取数据— Excel/CSV —打开文件

![](img/9fef017e998d56452ccafce7ad4a4e97.png)

**Connecting Data to PowerBI**

有时，一个文件中可能有多张幻灯片。因此，选择一个你打算去做的。始终选择 ***“转换数据”。*** *变换数据会把你带到* ***幂查询编辑器*** *。在此编辑器中，您将预处理数据集。那么为什么要选择转换数据呢，我们来看一个例子。*

![](img/09ba88f9ee43f434ab42592fe2254f97.png)

**Selecting Transform Data**

在上面的例子中，我们可以看到有一些空值，并且列标题没有正确放置。因此，我们必须在超级查询编辑器中修复它。

![](img/ec1fac6c6f49226134216ab0ab4e29be.png)![](img/eef7b302fe41215c038f76dc4755fb7e.png)![](img/822bb315813b1cff00aa4d5204800b29.png)![](img/54b9f94cc7b2baaaa40cdbf92b06bdff.png)

**Preparing the Data!**

在上面的例子中，我们删除了具有空值的行，使用第一行作为标题，还删除了充满空值的列。除此之外，我们还可以做****(如产品名称、周 ID)******【替换值】*** ***(如用其他值替换 null)***。这些是如何清理数据的一些基本示例。如果你看例子的右边，你可以看到 ***应用的步骤。如果你犯了任何错误，你随时可以去掉那一步，重新做。 ***有用的特性对吗？*******

> *创建数据、透视和取消透视*

*![](img/61dfbf7764a20166cef8b479ee3e829c.png)**![](img/4538d34cadaf55d28b8ec5d9bfa16ce0.png)**![](img/f673aed340180e904f5262f9e7606f22.png)*

***Create Data, Pivot, and Unpivot!***

*在这个例子中，我们在 power query 编辑器中创建了一个表来检查 pivot 和 unpivot 的特性！ ***Pivot 将第一列转换成标题(你必须选择)，其余的转换成特性。这个特性可以方便地放置你的数据集！ ***取消透视将帮助您正确地对数据进行分组(如右图)。*** 一个完美的用例是在属性列中调查问题，答案在值列中。****

## *数据建模:*

*数据建模是数据分析的核心功能之一。没有适当的数据建模，您将无法使用您的数据来正确地可视化和分析数据。因此，建模意味着根据数据之间的关系将数据连接在一起。*

*![](img/171e4a36e4d38f0f14fa2a145d0b3f63.png)*

*Before Modeling!*

*在本例中，我们已经上传了上述数据，并为数据建模阶段做好了准备。在数据建模阶段，我们将看到更多的例子。就这么办吧。首先，从表和列属性开始，你知道给它们适当的含义和描述。*

> *快速措施和新措施*

*![](img/172ea1c91857c4555072114e4a5d16a8.png)**![](img/8fe4a3d50e3d8d2d083c02296447edff.png)*

***Quick Measures!***

*快速测量有助于立即编写您自己的方程，以从数据中找出见解。新措施也与快速措施相同。但是，新的衡量标准要求您自己编写方程式，而快速衡量标准具有一些预定义的功能，您可以在下拉式表单中使用这些功能进行填充(例如左侧示例)。*

*![](img/0c0965e181600339bf435c4d523140a0.png)**![](img/443e23f6234b96fb1c653376176e2bd8.png)*

***New Measures!***

> *解决多对多关系:*

*在解析多对多关系的情况下，您必须创建一个只包含其他表或数据的唯一列的查找表。 ***另一个有用的功能是创建一组你的数据。选择您的数据并右键单击以创建一个组，并给它们一个合适的名称。****

*![](img/2955cbf39a472cccab5b35f0f8d4d947.png)*

***Grouping your data!***

*现在，我们想在地理位置和库存数据之间建立一种关系。 ***然而，现在他们却呈现出一种多对多对的关系。我们需要解决这个问题。我们将创建一个查找表。选择您的数据(如库存)，创建一个副本，并给它一个合适的名称。仅通过右键单击项目编号(基于该编号创建关系)选择唯一值，并删除重复行。从重复的数据中删除其他列！(查找表)。****

*![](img/ff09a17b6b8babe7b2e42183dbd84f38.png)*

***Lookup table!***

*现在，如果您单击“应用”并退出超级查询编辑器。您将能够使用产品信息查找表来查看地理数据和库存数据之间的关系(多对一)!*

*![](img/d24d549de7bea20a4d0d9c12f810abca.png)*

***Many to 1 Relationship (Data Modeling)***

> *创建层次结构*

*创建层次结构有助于对列进行分组，这有助于以后正确地显示数据。让我们看一个例子。*

*![](img/7dc6772eb2f0154ae1f9185ec0b7fbdd.png)**![](img/3b4a66c79149d312e4797c4640012f72.png)*

***Creating Hierarchy!***

> *使用 DAX 的行级安全性*

*这些措施给了我们一些限制，让我们知道谁可以在仪表板上看到什么！很有用的功能！*

*![](img/e7a1a9646789d9a1ac0774caa614cf52.png)**![](img/3ef2d91cc43fd6d9293a8a590d531f3e.png)**![](img/f656382c6dac41d09dd339cacbc3e0b5.png)*

***Row-Level Security!***

> *数据分析表达式*

*这项技术帮助我们在 PowerBI 中组合函数和操作符来构建公式和表达式。选择您的数据(如竞争价格)并点击新的衡量标准，输入以下等式:*

> *性价比= SUMX('竞争情报'，'竞争情报'[价格] —'竞争情报'[竞争对手价格] * '竞争情报'[售出单位])*

*![](img/5f882ecf02f05eb4ed04d9f11ac31976.png)*

***Dax Calculation Visualizations!***

*这些是对您的数据执行分析以决定如何对您的数据建模的一些直观示例！加上这些和其他计算，最终模型将是这样的:*

*![](img/ac8f5c490d8caab90b84f750038bdea6.png)*

***Data Modeling!***

## *数据可视化和分析:*

*完成数据建模后，现在我们可以开始数据可视化，以提取对数据的见解，看看它告诉我们什么！*

*![](img/607342a7fb39da37c242b6ff73bd49d6.png)**![](img/235538836346ff4a02f701f7dd14abb3.png)**![](img/896a485f68d3162d8a68c6ca68817e16.png)*

***Data Visualization!***

**在上面的例子中，我们使用了多种工具将数据可视化来讲述一个故事。这些功能包括时间序列分析、库存分析、使用条形图按类别计算销售额、显示总销售额和库存的 KPI 卡、显示哪些州在销售额方面做得好的地图，并且对于每个类别，我们已经按男性和女性属性可视化了调查。**

> *执行数据颜色的规则*

*![](img/440dc211b75af48e908e3c0ed8e52ef9.png)**![](img/424a8496ea5f8082cd130fa854660162.png)*

***Rules to visualize!***

*在上面的例子中，我们设置了规则，如果销售额少于 2000 万美元，就将类别的颜色改为橙色！*

> *斯莱伊萨*

*![](img/01a551ff58343b0483b477796969c2b0.png)**![](img/177a6a49d01ed23ced6a8fcefaf1dfa5.png)**![](img/2d72686566509ff2267910fe0db838c9.png)*

***Slicer Example!***

*我们可以使用类别切片器来查看条形图和地图中的总销售额！*

> *分析进展*

*我们可以使用分析按钮来使用趋势线、预测和发现异常等。*

*![](img/5fcc2ce7a4b302614a5ff21292ad5669.png)**![](img/caeeb2a1c3068c706ad212071e9e0697.png)*

***Analytics with tend line and forecast!***

*上图告诉我们，销售团队做得很棒！而且天气预报看起来也不错！*

> *库存数据的时间序列分析*

*![](img/226dfff9b54232d88f9a528c1e182063.png)**![](img/859eecf06d02fd2f94249ea826beaba9.png)**![](img/a369532880efda3db6bbb021bd3d1890.png)*

***Time Series Analysis!***

*除了所有这些特性，关键影响者(有助于发现异常值)和分解树有时也会有所帮助！两个例子都有！*

*![](img/65cfe319750ec532d82b8f2360f7f2e6.png)**![](img/412ce3add808c4cfd74c9aece15d1da7.png)*

***Key influencers and Decomposition Tree!***

*以上所有这些技术都有助于我们从数据中提取真知灼见，并根据已经发生的事情做出正确的商业决策？会发生什么？一个业务团队需要通过分析数据来找出这些答案！*

## *部署和维护交付*

*完成以上四个阶段后，现在该发表了！很简单！*

*![](img/3eea3640b4dce7204b1e02099eb1a11e.png)**![](img/23a339771b097814228b9716f354a16f.png)**![](img/d552577daa9183cf5742f558f1236ef6.png)*

***Publishing Procedure!***

*但是，有一些事情需要完成，即， ***安装一个网关，用于计划刷新、行级安全性、签署数据集、提供对数据集的访问、创建和管理工作区！*** 我图这个东西可以从这篇文章中分离出来，使之成为单独的一篇！*

*今天到此为止！干杯🙌*