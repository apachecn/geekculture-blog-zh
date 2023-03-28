# 在 Android 应用程序中使用 Excel 的简单方法

> 原文：<https://medium.com/geekculture/a-simple-way-to-work-with-excel-in-android-app-94c727e9a138?source=collection_archive---------12----------------------->

## 第 1 部分-编写 Excel 表格

![](img/5ae1ba1591ac15b46851b6a678613aad.png)

Copied from [More App](https://moreapp.com/en/blog/transform-excel-into-android-app/)

所以，在今天的文章中，你将学会用一种非常简单易行的方法在 Android 应用程序中编写 excel 表格。为此，我们需要从 [Apache POI](http://poi.apache.org/download.html) 添加库。

在应用程序级梯度文件中，添加以下依赖关系。

```
//Apache POI for excel
implementation 'com.github.SUPERCILEX.poi-android:poi:3.17'
compileOnly 'org.apache.poi:poi-ooxml:3.17'
```

## 先决条件

先前了解:
1-科特林
2-在 Android 中保存文件

## 编写 Excel 文件

> 创建 Excel 工作簿

要创建 excel 文件，我们首先需要从 XSSFWorkbook 创建工作簿，然后需要创建工作表以向其中添加数据。

您可以使用 workbook 的 *createSheet* 方法在单个工作簿中创建多个工作表。

> 创建工作表标题

在 excel 表格的第一行，通常人们会创建表格标题。因此，下面的代码将帮助您创建和理解 excel 表格中的标题。

单个单元格的最大列宽为 255 个字符，其整数值不能大于 65280。

> 标题样式

在上面的代码片段中，您可以为单元格背景或前景分配任何颜色。更多细节可以看 CellStyle 类中 setFillPattern 方法的实现。您还可以自定义单元格文本的字体样式，包括颜色。

> 添加数据

要在 excel 表中添加数据，您需要创建一行，并需要向该行的每个单元格添加数据。

> 创建单元格

创建具有行和列索引的单元格，并向其中添加值。

> 创建 Excel 文件

现在是时候从 Android 目录中的工作簿创建 excel 文件了。

快点！！在 Android 应用程序中创建和写入 excel 文件成功完成。

要阅读 excel 表格，请查看[第 2 部分](https://link.medium.com/ficYmmU0Ngb)。更新 excel 表格的下一部分将很快发布。请关注更多帖子和 Android 应用程序中问题的不同解决方案。