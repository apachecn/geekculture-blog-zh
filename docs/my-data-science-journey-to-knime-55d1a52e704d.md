# 我的 KNIME 数据科学之旅

> 原文：<https://medium.com/geekculture/my-data-science-journey-to-knime-55d1a52e704d?source=collection_archive---------4----------------------->

## 从 Excel 到 KNIME，通过 Power BI 和 Python。

![](img/4e6803cc33ec3705c76d142aa26af7ca.png)

Foto de [Josh Hild](https://www.pexels.com/es-es/@josh-hild-1270765?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) en [Pexels](https://www.pexels.com/es-es/foto/hombre-caminando-por-sendero-entre-arboles-2629233/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

这是一个会计经理如何成为数据分析师的故事。正如办公室里的每个人一样，我们使用并将继续使用 Excel，但由于更具体的需求，我开始研究这个未被充分利用的工具。

**微软 Power Query**
这个工具在 Excel 里面，让我们可以在 Excel 里面自动化提取、加载、转换数据库的过程。

 [## 关于 Excel 中的幂查询

### 使用 Power Query，您可以搜索数据源、建立连接，然后对数据进行整形(例如删除一个…

support.microsoft.com](https://support.microsoft.com/en-us/office/about-power-query-in-excel-7104fbee-9e62-4cb9-a02e-5bfb1a6c536a#:~:text=With%20Power%20Query%20%28called%20Get,to%20create%20charts%20and%20reports.) 

我们可以像使用 Excel 一样执行最基本的功能，或者更深入地研究该工具，使用其原生的 [M](https://docs.microsoft.com/en-us/powerquery-m/quick-tour-of-the-power-query-m-formula-language) 语言或标准的 [Python](https://www.python.org/) 或 [R](https://www.r-project.org/) 语言。

**微软 PowerPivot**
它的主要功能是对数据建模，建立关系，创建计算。

[](https://support.microsoft.com/en-us/office/power-pivot-overview-and-learning-f9001958-7901-4caa-ad80-028a6d2432ed) [## 动力中枢-概述和学习

### Power Pivot 是一种数据建模技术，允许您创建数据模型，建立关系，并创建…

support.microsoft.com](https://support.microsoft.com/en-us/office/power-pivot-overview-and-learning-f9001958-7901-4caa-ad80-028a6d2432ed) 

该工具主要使用 [DAX 语言](https://docs.microsoft.com/en-us/dax/)，这是一种为在 Power Pivot 中创建计算和测量列而创建的语言。

我在学习这两个工具时问自己的问题是什么时候用 Power Query，什么时候用 Power Pivot？

我可以告诉你我会怎么做，但是这两个工具太好了，太普遍了，没有单一的答案。

我将使用 Power Query 从各种来源加载数据，执行数据清理操作，甚至使用 m 执行简单的计算。一旦数据清理完毕，我将使用 Power Pivot 使用 DAX 语言执行计算。

我喜欢 Python(剧透，后面再说)，在 Power Query 中用于数据清理也是一个不错的选择。

**微软力量 BI** 这里事情变得更有趣了。我不会用几句话来命名这个工具，但是 Power Bi 是一个集成了 Microsoft Power Query、Microsoft Power Pivot 和其他功能的平台，是可视化的一个强大“部分”,毫无疑问，这是它的最佳附加值。

[](https://docs.microsoft.com/en-us/power-bi/fundamentals/power-bi-overview#:~:text=Power%20BI%20is%20a%20collection,on%2Dpremises%20hybrid%20data%20warehouses) [## 什么是 Power BI？-电源 BI

### Power BI 是软件服务、应用程序和连接器的集合，它们协同工作，将您不相关的资源…

docs.microsoft.com](https://docs.microsoft.com/en-us/power-bi/fundamentals/power-bi-overview#:~:text=Power%20BI%20is%20a%20collection,on%2Dpremises%20hybrid%20data%20warehouses) 

微软 Power BI 是 Tableau 的直接竞争对手，但目前它还没有与它开展项目，尽管当然，这是另一个选择，特别是在 Tableau 公共部分，你可以免费分享你的项目，这是创建你的投资组合非常有趣的事情。

[](https://www.tableau.com/) [## Tableau:商业智能和分析软件

### 没有单一的方式可以加速你的 Tableau 之旅，但是所有的路都通向 Tableau 社区。随着更多…

www.tableau.com](https://www.tableau.com/) 

**Python**
Python 是一种超级简单的跨平台编程语言，让你拥有极大的通用性，它的一个主要优势就是开源，也就是免费。

他钻研最多的书店，和所有数据科学家一样，在他最著名的书店，我会说:[熊猫](https://pandas.pydata.org/)。在 Pandas 中，我发现了**米托**工具，我已经在之前的文章中告诉过你了。

[](/geekculture/bye-bye-excel-hello-mito-cd7759ea1e56) [## 再见 Excel，你好米托

### 如果您可以编辑 Excel 文件，现在就可以编写 Python 代码。

medium.com](/geekculture/bye-bye-excel-hello-mito-cd7759ea1e56) 

当然，Python 也有机器学习库，在这里我们可以强调 Scikit-learn 或张量流，或数据可视化，在这里我将强调一个我喜欢的库: [Streamlit](https://streamlit.io/) 。

当我开始用 Python 编程时，我也不得不学习在哪里做，而最好的选择无疑是 Conda，不管是用 Jupyter Notebook，Jupyter Lab，Spyder…等等。

 [## 康达-康达文件

### 任何语言的包、依赖和环境管理——Python、R、Ruby、Lua、Scala、Java、JavaScript、C/ C++…

docs.conda.io](https://docs.conda.io/en/latest/) 

Python 总是被与 R 相比较，两者都是非凡的选择，为什么不兼容，但现在我会选择 Python，因为它的社区似乎已经领先，现在它给我们的选择比它给我们的多得多。请注意，R 也是一个很好的选项，尤其是对于统计计算或可视化。

KNIME
我最喜欢的工具，也是我目前花更多时间继续学习和钻研的地方。
[KNIME](https://www.knime.com/) 是一个工具，可以让你从头到尾做一个完整的数据科学项目，而这一切几乎不用接触代码，而且是免费的！
它允许你用一个仪表盘来完成提取、加载和转换、自动学习和整理的整个过程，而这一切都是通过节点和工作流可视化实现的。另一个不能不强调的点是它的社区。
如果你想知道它所有的优点，我推荐你阅读这篇文章:

[](/low-code-for-advanced-data-science/why-should-you-learn-knime-68c3f9142d4) [## 为什么要学 KNIME？

### 剧透警告:KNIME 是数据科学的民主化。

medium.com](/low-code-for-advanced-data-science/why-should-you-learn-knime-68c3f9142d4) 

如果你已经想开始学习它，这是我推荐的，读读这篇文章，我解释了我所遵循的过程，并且不花一分钱就能完成所有这些。

[](/low-code-for-advanced-data-science/how-i-learned-knime-100-free-1a9f0d3e2f8d) [## 我如何学习 KNIME | 100%免费

### 从自定进度的课程到书籍和视频，每个学习者都有适合自己的东西

中等。结论](/low-code-for-advanced-data-science/how-i-learned-knime-100-free-1a9f0d3e2f8d) 

**结论**

我已经选择了 KNIME 作为我的首选数据科学工具。

这并不意味着我不使用其他工具，例如 Python 或 Power Bi，但这是我花费时间最多的地方，因为似乎不可能同时掌握这些工具中的几个，因为它们是如此广泛和优秀，以至于很容易迷失方向。

我鼓励你调查、学习和发现最适合你的工具。

感谢您的阅读！

你可以在[推特](https://twitter.com/MoLa_data)或 [LinkedIn](https://www.linkedin.com/in/angel-molina-laguna/) 上找到我。