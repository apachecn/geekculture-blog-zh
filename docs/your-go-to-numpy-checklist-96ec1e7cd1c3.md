# 你的去 Numpy 清单

> 原文：<https://medium.com/geekculture/your-go-to-numpy-checklist-96ec1e7cd1c3?source=collection_archive---------19----------------------->

快速浏览所有重要的 numpy 命令！

![](img/c87fc4b327ae19d61dee4ff2b712a888.png)

Photo by [Bench Accounting](https://unsplash.com/@benchaccounting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/online-classes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Numpy](http://www.numpy.org/) 是 Python 中科学计算的核心库。它提供了一个高性能的多维数组对象，以及处理这些数组的工具。

# 数组

numpy 数组是由相同类型的值组成的网格，由一组非负整数索引。维数是数组的*秩*；数组的*形状*是一个整数元组，给出了数组在每个维度上的大小。

```
import numpy **as** np

a **=** np.array([1242, 213, 312321])   
*# Create a rank 1 array* **print**(type(a))            
*# Prints "<class 'numpy.ndarray'>"* **print**(a.shape)            
*# Prints "(3,)"* **print**(a[0], a[1], a[2])   
*# Prints "1242 213 312321"* a[0] **=** 5                  
*# Change an element of the array* **print**(a)                  
*# Prints "[5, 213, 312321]"* 
b **=** np.array([[1,2,3],[4,5,6]])    *# Create a rank 2 array* **print**(b.shape)                     *# Prints "(2, 3)"* **print**(b[0, 0], b[0, 1], b[1, 0])   *# Prints "1 2 4"*
```

Numpy 还提供了许多创建数组的函数:

```
import numpy **as** np

a **=** np.zeros((2,2))   *# Create an array of all zeros* **print**(a)              *# Prints "[[ 0\.  0.]
*                      *[ 0\.  0.]]"* 
b **=** np.ones((1,2))    *# Create an array of all ones* **print**(b)              *# Prints "[[ 1\.  1.]]"* 
c **=** np.full((2,2), 7)  *# Create a constant array* **print**(c)               *# Prints "[[ 7\.  7.]
*                       *         [ 7\.  7.]]"*
```

# 数组索引

Numpy 提供了几种方法来索引数组。

**切片:**类似于 Python 列表，numpy 数组也可以切片。由于数组可能是多维的，因此必须为数组的每个维度指定一个切片:

```
# slice single item 
import numpy as np 

a = np.arange(10) 
b = a[5] 
print b
```

NumPy 包包含一个迭代器对象 **numpy.nditer** 。这是一个高效的多维迭代器对象，使用它可以遍历一个数组。使用 Python 的标准迭代器接口访问数组的每个元素。

```
print 'Modified array is:'
for x in np.nditer(a):
   print x**Output:**
Modified array is:
0 5 10 15 20 25 30 35 40 45 50 55
```

NumPy 有相当多有用的统计函数，用于查找最小值、最大值、百分位标准偏差和方差等。从数组中的给定元素。这些功能解释如下

> `[**ptp**](https://numpy.org/doc/stable/reference/generated/numpy.ptp.html#numpy.ptp)` (a[，axis，out，keepdims])
> 沿轴的数值范围(最大值-最小值)。
> 
> `[**percentile**](https://numpy.org/doc/stable/reference/generated/numpy.percentile.html#numpy.percentile)` (a，q[，轴，出，...])
> 沿指定轴计算数据的第 q 个百分点。
> 
> `[**nanpercentile**](https://numpy.org/doc/stable/reference/generated/numpy.nanpercentile.html#numpy.nanpercentile)` (a，q[，轴，出，...])
> 沿指定轴计算数据的第 q 个百分位数，同时忽略 nan 值。
> 
> `[**quantile**](https://numpy.org/doc/stable/reference/generated/numpy.quantile.html#numpy.quantile)` (a，q[，轴，输出，覆盖 _ 输入，...])
> 沿指定轴计算数据的第 q 个分位数。
> 
> `[**nanquantile**](https://numpy.org/doc/stable/reference/generated/numpy.nanquantile.html#numpy.nanquantile)` (a，q[，轴，出，...])
> 沿指定轴计算数据的第 q 个分位数，同时忽略 nan 值

# NumPy 数组操作函数

NumPy 模块的数组操作功能帮助我们对数组元素进行修改。

看看下面的功能——

*   [numpy . shape()](https://www.askpython.com/python-modules/numpy/python-numpy-reshape-function):这个函数允许我们在不影响数组值的情况下改变数组的维数。
*   numpy.concatenate():以行或列的方式连接两个相同形状的数组。

# NumPy 字符串函数

使用 NumPy 字符串函数，我们可以操作数组中包含的字符串值。下面提到了一些最常用的字符串函数:

*   `numpy.char.add() function`:连接两个数组的数据值，合并它们，并作为结果表示一个新的数组。
*   `numpy.char.capitalize() function`:将整个单词/字符串的第一个字符大写。
*   `numpy.char.lower() function`:将字符串字符的大小写转换为小写字符串。

# 数学函数

NumPy 包含了大量的各种数学运算。NumPy 提供标准三角函数、算术运算函数、处理复数等。

[numpy.sin(x[，out]) = ufunc 'sin')](https://www.geeksforgeeks.org/numpy-sin-python/) :这个数学函数帮助用户计算所有 x(作为数组元素)的三角正弦。

[numpy.cos(x[，out]) = ufunc 'cos')](https://www.geeksforgeeks.org/numpy-cos-python/) :这个数学函数帮助用户计算所有 x(作为数组元素)的三角余弦。

# 奖金！np .剪辑

函数`[clip](https://numpy.org/doc/stable/reference/generated/numpy.clip.html)`用于将数组的值保持在一个定义的上限和下限之间的区间内。

```
*# Example-41: 1D array* 
x = np.array([1,0,3,2,4,9,8])

print(x.clip(3, 5))
[3 3 3 3 4 5 5]
```

如果你卡住了，一定要伸出手来评论！

其他可能感兴趣的文章:

*   [熊猫 10 分钟指南。这将作为获得… |的基本指南，由 Sam |极客文化| 2022 年 1 月| Medium](/geekculture/pandas-10-minute-guide-31dc26a874f7)
*   [Apache Spark-I 入门|作者 Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-i-5fbbe7b47667)
*   细流企鹅和帕尔默企鹅。上周在网飞疯狂狂欢… |作者:山姆|极客文化|媒体

干杯，请关注更多此类内容！:)

如果你喜欢它的内容，你现在也可以给我买一杯咖啡！
[samunderscore12 正在创作数据科学内容！(buymeacoffee.com)](https://www.buymeacoffee.com/samunderscore12)