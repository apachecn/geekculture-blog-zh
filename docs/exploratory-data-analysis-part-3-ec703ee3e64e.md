# 单线探索性数据分析

> 原文：<https://medium.com/geekculture/exploratory-data-analysis-part-3-ec703ee3e64e?source=collection_archive---------39----------------------->

在阅读完[第一部分](https://prasantdixit.medium.com/exploratory-data-analysis-eda-44912e492879)和[第二部分](https://prasantdixit.medium.com/exploratory-data-analysis-part-2-2e68180be41e)之后，你一定观察到了我们用来探索数据集的模式。为了完全理解数据集，我们必须遵循大量的步骤。但是我们也必须记住它们的顺序。

有一些 Python 库在使用它们，你可以在 **1** **行**执行 **EDA。对，你没看错。整个 EDA 也可以在中下载。html 格式，也可以在浏览器中看到。**

执行 EDA 的 Python 库如下

1.  数据准备库
2.  sweetviz 图书馆
3.  熊猫 _ 剖析库
4.  autoviz 库
5.  数据图书馆

![](img/72397928e29b0a042e11dbf223875c9a.png)

现在让我们开始看看需要为它实现的代码。

1.  **数据准备**

数据准备。EDA 是 Python 中最快最简单的 EDA(探索性数据分析)工具。dataprep 的[文档](https://pypi.org/project/dataprep/)中规定的 **//** 。

```
#to install dataprep in Notebook
!**pip install** dataprep#to see EDA in notebook
**from** dataprep.eda **import** plot
plot(data)   #for plotting EDA in notebook#to see report on Browser and for downloading .html file **from** dataprep.eda **import** create_report
report= create_report(data)
report.show_browser()  #to see it on Browser
report.save(filename='/EDA-Report') #to download
```

访问**数据准备**的[文档](https://pypi.org/project/dataprep/)以了解更多信息。

2.sweetviz

Sweetviz 是一个开源 Python 库，只需两行代码，就能为 kickstart **EDA** (探索性数据分析)**生成漂亮的高密度可视化效果。输出是一个完全独立的 HTML 应用程序。//如 sweetviz 的[文档](https://pypi.org/project/sweetviz/)所述。**

```
#to install sweetviz in Notebook
!**pip install** sweetviz
**import** sweetviz#to analyse dataset
report= sweetviz.analyse([data, 'dataset'], target_feat= 'Target_col')
report.show_html('EDA-Report.html') #to see it in html format#to compare datasets
report_1= sweetviz.compare([train_data, 'train'], [test_data, 'test], target_feat= 'Target_col')
report.show_html('EDA-Report-1.html') #to see comparison in html format
```

请访问 **sweetviz** 的[文档](https://pypi.org/project/sweetviz/)了解更多信息。

3.**熊猫 _ 简介**

这个功能很棒，但是对于严肃的探索性数据分析来说有点基础。`pandas_profiling`用`df.profile_report()`扩展熊猫数据框，进行快速数据分析。//在 pandas_profiling [文档](https://pypi.org/project/pandas-profiling/)中声明。

```
#to install pandas_profiling in Notebook
!**pip install** pandas-profiling
**from** pandas_profiling **import** ProfileReport#to create report using ProfileReport
report= ProfileReport(data, title='Report', explorative= True)report.to_widgets() #to display in Notebook
report.to_file('Report.html') # to create .html file
```

访问**熊猫 _ 简介**的[文档](https://pypi.org/project/pandas-profiling/)了解更多信息。

4.**自动对焦**

用一行代码自动可视化任何大小的任何数据集。//在 autoviz 的[文档](https://pypi.org/project/autoviz/)中声明。

```
#to install autoviz in Notebook
!**pip install** autoviz
**from** autoviz.AutoViz_Class **import** AutoViz_Classav = AutoViz_Class() #initiated AutoViz_Class
report= av.AutoViz(filename='file_name.csv',sep=',',depVar='Target_Var')
#this will plot EDA on your notebook
```

访问 **autoviz** 的[文档](https://pypi.org/project/autoviz/)了解更多信息。

5. **dtale**

D-Tale 是 Flask 后端和 React 前端的结合，为您提供了一种查看和分析 Pandas 数据结构的简单方法。它与 ipython 笔记本和 python/ipython 终端无缝集成。目前，这个工具支持像 DataFrame、Series、MultiIndex、DatetimeIndex & RangeIndex 这样的 Pandas 对象。//在 D-tale [文档](https://pypi.org/project/dtale/)中说明

```
#install dtale in Notebook
!**pip install** dtale
**import** dtale**import** dtale.app **as** dtale_app #to launch dtale webapp
dtale_app.USE_COLAB = **True** #when implemeting on GOOGLE Colab
dtale.show(data) #this gives link of webapp 
```

访问 **autoviz** 的[文档](https://pypi.org/project/dtale/)了解更多信息。

这些是一些使用它们的库，你可以在 **1 行**中做 EDA。你一定很好奇使用它们会得到什么样的结果。使用它们并给我你的反馈。除此之外，你必须有关于自己做 EDA 的知识，你可以从我写的 EDA 的 [Part1](https://prasantdixit.medium.com/exploratory-data-analysis-eda-44912e492879) 和 [Part2](https://prasantdixit.medium.com/exploratory-data-analysis-part-2-2e68180be41e) 中学习。