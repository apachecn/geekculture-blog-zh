# #DS 02。熊猫，数据框和情节

> 原文：<https://medium.com/geekculture/ds-02-pandas-dataframes-and-plot-cb7751760e6d?source=collection_archive---------61----------------------->

在我的上一个 medium 故事中，我讲述了我是如何发现数据科学世界的，在接下来的帖子中，我将描述我对它的了解。

=>Gihub 资源库<=
[](https://github.com/saulotp/ds02)

*我正在集中精力学习 python 成为一门简单但强大的语言，事实上，我相信 python 是编程世界的“入门”。在寻找数据科学中关于 python 的学习资料时，我发现了 Spyder 和 Jupyter，它们是用于数据分析的非常棒的工具。此外，这些工具将帮助我展示我的项目的进展。*

*首先我将讲述这个项目。我从网上的某个地方得到了一个 excel 文件(该文件将放在 GitHub 存储库中)，这个文件包含了分散在巴西的一些商场的销售数据。有了这些数据，我想回答一些问题，比如:
-哪个商场的销售额更多
-所有商场的总销售额是多少
-所有商场的销售额的平均值是多少
-验证某个日期间隔内的一些数据
-绘制一些数据*

*首先，我必须导入 pandas 库并让 python 读取。xlsl 文件:*

*![](img/d62713f897010b2fc5a94e305bef8a7a.png)*

*读取 excel 文件并创建数据框架“dfmain”后，我们可以打印以查看结果:*

*![](img/5013cdd32102eca4691c15a181e9d800.png)*

*我们还可以使用命令“display”来设置输出的样式(看起来更酷，我将使用 display 而不是 print):*

*![](img/6a0f6c5dda47d7982dbade573b894009.png)*

*在下面的代码中，我将日期列从 string 转换为 pandas 认为是“date”的格式。之后，创建了一个新的数据框架，只显示 ID mall 和每个 mall 的所有销售额的总值，按降序排列。*

*![](img/c8c16e75fdece21509e55b59784567e2.png)*

*结果如下:*

*![](img/04fdfad5e1d7d687ec42666d5ddf787f.png)*

*The table shows us that the mall with more sales value was ‘Shopping Vila Velha’ and the worst was ‘Shopping Morumbi’*

*接下来，我想知道所有商场的所有销售额的总和，然后我得到一个方法，可以对一个或多个列求和，并对“Valor Final”列求和。之后，我在 DataFrame 上创建了另一列，以百分比格式显示值。*

*![](img/d37181612197a1e28423410f78e91adc.png)**![](img/ac4fc408690b2086e1dfe70dd91e57f6.png)*

*为了回答关于销售平均值的问题，我使用了相同的代码，但使用了“mean()”方法:*

*![](img/94ce106c5ae13f0340ba6e8c6e6da367.png)**![](img/235a78d4502bc799209fda559bc29cf7.png)*

*在下一节中，我决定编写一个脚本，只需更改['ID Loja'] == 'Selected Mall '，就可以提取每个商城的数据*

*![](img/3bc28ee0d7fe59d551f725afe31a0d73.png)*

*为了知道每件产品卖出了多少，我从(dfmall)创建了另一个数据框架。*

*![](img/3f1d438742bded00e60555af1ee1400c.png)*

*This table shows that the product with more value sales was ‘Terno Linho’ with a total value of ‘R$ 60.000,00’ representing 4,08% of all product sales from this mall.*

*如果我们想要分析一个特定的日期周期，我们可以创建一个过滤器来确定日期周期，然后更精确地提取数据。通过下面的代码，我发现哪种产品在一月份销售得更多，但是日期可以在任何时间段更改，例如:从“2019-02-18”到“2019-07-23”*

*![](img/c27cb1c0b3a0b681eec1fab1f4adba1f.png)*

*因此，我们有:*

*![](img/51737562a58b19bd6cdbdbe731b0467a.png)*

*The table above shows us that in January, the item that was more sold was “Terno” with 6,4% of all sales. And the item with fewer sales was “Chinelo Liso” with 0,04%*

*要查看每月的总销售额，我们可以使用命令“重采样(参数)”，并选择 M(月)作为“数据”(日期)列中的参数。因此，我们将使用选定的数据创建另一个数据框架:*

*![](img/b25102910e8d0c71886ac841a44924b3.png)*

*In this case, I removed the index “Valor Total” because I have encountered some errors when I’m trying to plot =/*

*![](img/24518fa081208dd4248859b766e560c4.png)*

*This table shows the total value of all sales for each month*

*最后是剧情时间。为了绘图，我们必须导入另一个 python 库:*

*![](img/fddeed9f0397b8cc5f5a77d9427c5296.png)*

*Full line code: “pt = dfmalldatessumlsales.plot(style=’.-’, ms=15, color=’red’, figsize=(20,10), lw=5, legend=False, xlabel=’’, title = ‘Total Sales’)”*

*因此，我们有一个图像来表示每个月所有销售额的数据:*

*![](img/306747edb154e032534191fe0a0a83e2.png)*

*With this image, we can observe that the month with more value sales was May and the month with fewer sales was August.*

*我们可以用同样的方法来绘制销售的平均值:*

*![](img/bcc5e442a3f04aa3704e4f1126c1b461.png)**![](img/c6cf1fa8a5699c6912e1e0803ebfd7e4.png)*

*Mean of sales per month*

*嗯，“就是这个”。*

*一个人学习有时会很难，但这是可能的。
现在，回答前面的问题:
问:哪个商场的销售额更高？
答:购物维拉韦拉 1.615.271 元人民币*

*问:所有商场的总销售额是多少？答:38 . 959 . 752 . 00 雷亚尔*

*问:所有来自商场的销售意味着什么？
答:1 558 390.08 雷亚尔*

*问:核实整个日期区间的一些数据
答:2019-01-01 至 2019-01-31 年 1 月，shopping Morumbi 出售给“Terno”的总价值为 9.100，00 雷亚尔。*

*问:绘制一些数据
答:*

*![](img/306747edb154e032534191fe0a0a83e2.png)*

*=>Gihub 资源库<=
[***https://github.com/saulotp/ds02***](https://github.com/saulotp/ds02)*

*联系我:
-saulodetp@gmail.com
-Ig:saulodetp
-Linkedin:saulodetp*