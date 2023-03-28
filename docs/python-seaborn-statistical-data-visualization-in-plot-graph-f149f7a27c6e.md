# 如何使用 Seaborn Python 库制作统计数据可视化的交互式绘图

> 原文：<https://medium.com/geekculture/python-seaborn-statistical-data-visualization-in-plot-graph-f149f7a27c6e?source=collection_archive---------3----------------------->

By- SANDEEP KUMAR PATEL

![](img/37ae5cd060125947023afc09877a431f.png)

# 海生的

Seaborn 是一个基于 matplotlib 的 Python 数据可视化库。它提供了一个高层次的界面来绘制有吸引力的和信息丰富的统计图形。
关于这个库背后的思想的简要介绍，你可以阅读介绍性笔记。请访问安装页面，了解如何下载该软件包并开始使用。您可以浏览示例库，看看可以用 seaborn 做些什么，然后查看教程和 API 参考来了解如何做。
要查看代码或报告错误，请访问 GitHub 资源库。一般支持问题在 Stackoverflow 或 discourse 上最常见，这些网站为 seaborn 提供了专门的渠道。

在这篇文章中，我们试图创建一个绘图各种选项在 seaborn 图书馆提供的最有效的方式来绘图和可视化我们的数据离开

# 带误差带的时间序列图

**seaborn 使用的组件:**`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)``[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)``[**lineplot()**](https://seaborn.pydata.org/generated/seaborn.lineplot.html#seaborn.lineplot)`

![](img/1e81c870f790740c126ab3caaab6c6d7.png)

# 具有连续色调和大小的散点图

**使用的海鹏组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**cubehelix_palette()**](https://seaborn.pydata.org/generated/seaborn.cubehelix_palette.html#seaborn.cubehelix_palette)`、`[**relplot()**](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)`

![](img/488d5cb7d624826c26234679090396d9.png)

# 小型多重时间序列

**使用的海鹏组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**relplot()**](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)`、`[**lineplot(**](https://seaborn.pydata.org/generated/seaborn.lineplot.html#seaborn.lineplot)`

![](img/fd12815dceffce24dcd872fd7d800f4a.png)![](img/8e974825ee8b1091405cb7763d8c555a.png)

# 带观察值的水平箱线图

**使用的海博组件:**、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**boxplot()**](https://seaborn.pydata.org/generated/seaborn.boxplot.html#seaborn.boxplot)`、`[**stripplot()**](https://seaborn.pydata.org/generated/seaborn.stripplot.html#seaborn.stripplot)`、`[**despine()**](https://seaborn.pydata.org/generated/seaborn.despine.html#seaborn.despine)`

![](img/429d4f9033edcced0473ce42a1a92865.png)

# 边缘分布的线性回归

**seaborn 组件使用:**`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)``[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)``[**jointplot()**](https://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot)`

![](img/1e5dc6af5ade85d414d9fe5d277a843c.png)

# 具有不同点大小和色调的散点图

**使用的海鹏组件:** `[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**swarmplot()**](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot)`

![](img/5f80e5c76dd2b86cdae4abfb4531c932.png)

# 分类变量散点图

**使用的 seaborn 组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**swarmplot()**](https://seaborn.pydata.org/generated/seaborn.swarmplot.html#seaborn.swarmplot)`

![](img/1fe569f89c95f25ba0e4e02d2a7e11fc.png)

# 带观察的小提琴图

**seaborn 使用的组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**violinplot()**](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot)`

![](img/50c89ef3df861dcd3e98cee21b540018.png)

# 具有边缘直方图的平滑核密度

**seaborn 使用的组件:**`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)``[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)``[**JointGrid**](https://seaborn.pydata.org/generated/seaborn.JointGrid.html#seaborn.JointGrid)`

![](img/eb76e39a6c0f607e89f26856f9ea0e2e.png)

# 带注释的热图

**使用的海鹏组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**heatmap()**](https://seaborn.pydata.org/generated/seaborn.heatmap.html#seaborn.heatmap)`

![](img/853cddbb4d2a76a91644a8d4c572e55c.png)

# 绘制大型分布图

**使用的 seaborn 组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**boxenplot()**](https://seaborn.pydata.org/generated/seaborn.boxenplot.html#seaborn.boxenplot)`

![](img/10a253c64137d7196bedc1c1f24a41e5.png)

# 具有不同点大小和色调的散点图

**seaborn 组件使用:**`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)``[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)``[**relplot()**](https://seaborn.pydata.org/generated/seaborn.relplot.html#seaborn.relplot)`

![](img/b7b0b9cc9d9c900df95a89c5b1d024d8.png)

# 对数标度的堆积直方图

**使用的海鹏组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**despine()**](https://seaborn.pydata.org/generated/seaborn.despine.html#seaborn.despine)`、`[**histplot()**](https://seaborn.pydata.org/generated/seaborn.histplot.html#seaborn.histplot)`

![](img/4002cdeb7bfd3a498d1a4c5a96bbfebe.png)

# 配对分类图

**使用的 seaborn 组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**PairGrid**](https://seaborn.pydata.org/generated/seaborn.PairGrid.html#seaborn.PairGrid)`、`[**despine()**](https://seaborn.pydata.org/generated/seaborn.despine.html#seaborn.despine)`

![](img/58240565a27e3d5fff69fe96aff8c0ed.png)

# 在大量面上绘图

**seaborn 使用的组件:** `[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`，`[**FacetGrid**](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid)`

![](img/f131def5c60d076d3d840c1c22e0244e.png)

# 宽格式数据集中的紫色图

**使用的海博组件:**、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**violinplot()**](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot)`、`[**despine()**](https://seaborn.pydata.org/generated/seaborn.despine.html#seaborn.despine)`

![](img/ea9e315476da4d3f824844bde0a93a33.png)![](img/27e7558abc718e8ec6659f4532d2d5ef.png)

# 带观察的小提琴图

**seaborn 使用的组件:**、`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)`、`[**violinplot()**](https://seaborn.pydata.org/generated/seaborn.violinplot.html#seaborn.violinplot)`

![](img/29586181025eb9b58885b1b32530b07c.png)

# 多元素二元图

**使用的海鹏组件:**`[**set_theme()**](https://seaborn.pydata.org/generated/seaborn.set_theme.html#seaborn.set_theme)``[**scatterplot()**](https://seaborn.pydata.org/generated/seaborn.scatterplot.html#seaborn.scatterplot)``[**histplot()**](https://seaborn.pydata.org/generated/seaborn.histplot.html#seaborn.histplot)``[**kdeplot()**](https://seaborn.pydata.org/generated/seaborn.kdeplot.html#seaborn.kdeplot)`

![](img/f83d270d312056b5543270168b1ff8c4.png)

# 条件核密度估计

**使用的海博组件:**、`[**load_dataset()**](https://seaborn.pydata.org/generated/seaborn.load_dataset.html#seaborn.load_dataset)`、`[**displot()**](https://seaborn.pydata.org/generated/seaborn.displot.html#seaborn.displot)`

![](img/d9b9384adde04943548d7bc41976fa45.png)

**了解更多详情-**

[](https://seaborn.pydata.org/) [## seaborn:统计数据可视化——seaborn 0 . 11 . 1 文档

### Seaborn 是一个基于 matplotlib 的 Python 数据可视化库。它为绘图提供了一个高级接口…

seaborn.pydata.org](https://seaborn.pydata.org/) ![](img/50a69f62b315af74f9e224bbb3d0f1b7.png)

**感谢您抽出时间……..！！！**