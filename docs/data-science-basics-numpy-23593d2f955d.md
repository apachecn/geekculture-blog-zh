# 数据科学基础:Numpy

> 原文：<https://medium.com/geekculture/data-science-basics-numpy-23593d2f955d?source=collection_archive---------17----------------------->

## NumPy 入门

![](img/47a6468da6dab0bd7dffefaf2c200852.png)

Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# Numpy

[Numpy](https://numpy.org/) 是一个流行于数据科学社区的 python 库。它可以用于多维大数组操作。它还用于数组的数学运算。

Numpy 是有用的，因为它容易学习，高效，并具有很高的计算能力。

要使用 NumPy，我们首先必须导入它。我们使用一个更短的别名`np`来使用 numpy。

```
import numpy as np
```

## Numpy 数组创建

Numpy 数组是同构的数据结构，数组中的数据必须是相同的数据类型。

Numpy 数组可以从列表中创建。

```
>>> a_list = [1,2,3,4,5,6,7,8,9]
>>> a_arr=np.array(a_list)
>>> print(a_arr)
[1 2 3 4 5 6 7 8 9]
```

如果列表不是同类的，则向上转换项目以匹配数据类型。

```
>>> a_list = [1,2,3.9]
>>> a_arr=np.array(a_list)
>>> print(a_arr)
[1\.  2\.  3.9]
>>> print(a_arr.dtype)
float64
>>> a_list = [1,'2',3.9]
>>> a_arr=np.array(a_list)
>>> print(a_arr)
['1' '2' '3.9']
>>> print(a_arr.dtype)
<U21
```

还有其他方法来创建 numpy 数组，如`np.ones`、`np.zeros`、`np.random.random`、`np.arange`、`np.linspace`等。

```
>>> print(np.ones((4,3)))
[[1\. 1\. 1.]
 [1\. 1\. 1.]
 [1\. 1\. 1.]
 [1\. 1\. 1.]]
>>> print(np.zeros((4,3)))
[[0\. 0\. 0.]
 [0\. 0\. 0.]
 [0\. 0\. 0.]
 [0\. 0\. 0.]]
>>> print(np.ones((4,3), dtype=np.int))
[[1 1 1]
 [1 1 1]
 [1 1 1]
 [1 1 1]]
>>> print(np.random.random((2,3)))
[[0.7386583  0.02779194 0.97240368]
 [0.58384794 0.78786521 0.20495265]]
>>> print(np.arange(3))
[0, 1, 2]
>>> print(np.arange(3.0))
[ 0.,  1.,  2.]
>>> print(np.arange(3,7))
[3, 4, 5, 6]
>>> print(np.linspace(2.0, 3.0, num=5))
[2\.   2.25 2.5  2.75 3\.  ]
>>> print(np.linspace(2.0, 3.0, num=5, endpoint=False))
[2\.  2.2 2.4 2.6 2.8]
```

正如我们在上面的例子中看到的，我们可以在一个元组中指定数组维数来创建数组。还有其他类似`ones_like`、`zeros_like`等方法。它将一个数组作为输入，并返回一个具有相同维数的数组。

```
>>> a=np.ones((4,3))
>>> b=np.zeros_like(a)
>>> print(b)
[[0\. 0\. 0.]
 [0\. 0\. 0.]
 [0\. 0\. 0.]
 [0\. 0\. 0.]]
```

numpy 中还有一个有用的方法`tile`。它将一个数组作为输入，并重复多次。

```
>>> print(np.tile([0,1,2],3))
[0 1 2 0 1 2 0 1 2]
>>> print(np.tile(np.array([0,1,2]),3))
[0 1 2 0 1 2 0 1 2]
```

它接受数组、列表或集合作为输入。

## Numpy 数组属性

numpy 数组的一些属性有助于我们理解它的结构和内容。以下是其中的一些例子。

```
>>> a=np.ones((4,3))
>>> a.shape
(4, 3)
>>> a.size # total no of elements in the array
12
>>> a.ndim # number of dimensions
2
>>> a.dtype
dtype('float64')
```

## NumPy 数组上的操作

基于元素的操作可以使用运算符来完成。

```
>>> print(np.array([[0,1],[2,3]])+np.array([[7,8],[5,6]]))
[[7 9]
 [7 9]]
```

对于两个数组之间的元素操作，它们应该有相似的维数，否则我们会得到一个错误。

```
>>> print(np.array([[0,1],[2,3]])+np.array([[7,8],[5,6],[3,4]]))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: operands could not be broadcast together with shapes (2,2) (3,2)
```

我们也可以用运算符对常数进行加/乘或其他运算。

```
>>> print(np.array([[0,1],[2,3]])**3)
[[ 0  1]
 [ 8 27]]
>>> print(np.array([[0,1],[2,3]])/3)
[[0\.         0.33333333]
 [0.66666667 1\.        ]]
```

## 数组操作

我们可以使用`reshape`来改变一个数组的维数。

```
>>> print(np.arange(6))
[0 1 2 3 4 5]
>>> print(np.arange(6).reshape(3,2))
[[0 1]
 [2 3]
 [4 5]]
```

一个更有趣的技巧是，如果我们不知道其中一个维度，我们可以用-1 离开 numpy 来计算未知的维度。

```
>>> print(np.arange(9).reshape(3,-1))
[[0 1 2]
 [3 4 5]
 [6 7 8]]
```

阵列可以水平或垂直堆叠在一起。要垂直堆叠数组，两个数组的列数应该相同。类似地，水平堆叠的行数应该相同。

```
>>> a1=np.arange(12,24).reshape(3,4)
>>> a1
array([[12, 13, 14, 15],
       [16, 17, 18, 19],
       [20, 21, 22, 23]])
>>> a2=np.arange(9).reshape(3,-1)
>>> a2
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])
>>> a3=np.arange(14,28).reshape(-1,7)
>>> a3
array([[14, 15, 16, 17, 18, 19, 20],
       [21, 22, 23, 24, 25, 26, 27]])>>> a12=np.hstack((a1,a2))
>>> a123=np.vstack(a12,a3)
>>> a12
array([[12, 13, 14, 15,  0,  1,  2],
       [16, 17, 18, 19,  3,  4,  5],
       [20, 21, 22, 23,  6,  7,  8]])
>>> a123
array([[12, 13, 14, 15,  0,  1,  2],
       [16, 17, 18, 19,  3,  4,  5],
       [20, 21, 22, 23,  6,  7,  8],
       [14, 15, 16, 17, 18, 19, 20],
       [21, 22, 23, 24, 25, 26, 27]])
```

数组可以转置。

```
>>> x = np.array([[1,2],[3,4]]) 
>>> x
array([[1, 2],
       [3, 4]])
>>> x.T
array([[1, 3],
       [2, 4]])
```

## 数组上的数学运算

我们可以对整个数组进行数学运算。

```
>>> a = np.array([[1,2],[3,4]]) 
>>> print(np.sin(a))
[[ 0.84147098  0.90929743]
 [ 0.14112001 -0.7568025 ]]
>>> print(np.cos(a))
[[ 0.54030231 -0.41614684]
 [-0.9899925  -0.65364362]]
>>> print(np.exp(a))
[[ 2.71828183  7.3890561 ]
 [20.08553692 54.59815003]]
```

如果我们需要将用户定义的函数应用于数组，我们可以遍历数组并将函数应用于每个元素。但是，有一种有效的矢量化方法可以做到这一点。

```
>>> f = np.vectorize(lambda x: x/(x+1))
>>> f(a)
array([[0.5       , 0.66666667],
       [0.75      , 0.8       ]])
```

我们可以创建一个函数，让`vectorize`方法处理剩下的部分。

## 阵列上的线性代数运算

我们可以在数组上执行基本的线性代数运算。NumPy 的内置包`np.linalg`允许我们应用线性代数运算。

```
>>> np.linalg.inv(a)
array([[-2\. ,  1\. ],
       [ 1.5, -0.5]])
>>> np.linalg.det(a)
-2.0000000000000004
>>> np.linalg.eig(a) # eigen vector and eigen values
(array([-0.37228132,  5.37228132]), array([[-0.82456484, -0.41597356],
       [ 0.56576746, -0.90937671]]))
```

可以使用`np.dot`方法进行矩阵乘法。

```
>>> np.dot(a, np.linalg.inv(a))
array([[1.00000000e+00, 1.11022302e-16],
       [0.00000000e+00, 1.00000000e+00]])
```

Numpy 是帮助我们进行数据分析的有用工具。在某种程度上，它是数据科学实践方面的一个支柱。

要了解更多关于 Numpy 的信息，点击[这里](https://numpy.org/doc/stable/user/tutorials_index.html)。

希望你喜欢。敬请关注更多内容。