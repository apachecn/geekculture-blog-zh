# 机器学习的 NumPy 完全指南

> 原文：<https://medium.com/geekculture/a-complete-guide-on-numpy-for-machine-learning-fd4ec1f168b7?source=collection_archive---------10----------------------->

![](img/80fee28927c6bd1bdd888aba62eb3115.png)

Photo by [Mohammad Rahmani](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 目录

1.  [什么是 NumPy](http://77d3)
2.  [如何安装 NumPy](http://1a3e)
3.  【NumPy 入门
4.  [数组创建例程](http://92bf)
5.  [数组操作例程](http://3361)
6.  [二元运算](http://a1ea)
7.  [数学函数](http://e8e8)
8.  [线性代数](http://003a)

# ***什么是 NumPy？***

NumPy 是一个 Python 库，允许你处理多维数组、线性代数、统计运算等等。NumPy 也是数字 T21 的意思。Numpy 为我们提供了各种简化 Python 数据处理的特性。

# 如何安装 NumPy？

![](img/8449aad0fac0ce728e694f0e8d588ce4.png)

Photo by [Crissy Jarvis](https://unsplash.com/@crissyjarvis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用 pip 安装 Numpy:

```
 pip install numpy
```

使用 Anaconda 安装 Numpy:

```
conda install numpy
```

# 【NumPy 入门

## **导入库:**

```
**>>>** import **numpy** as **np**
```

## **NumPy 基础:**

‣ 让我们创建一个 python 列表并将这个列表转换成数组

```
**>>>** nums = [[1, 2, 3], [4, 5, 6]]  *#2D Python list*
**>>>** nums
[[1, 2, 3], [4, 5, 6]]**>>>** arr = np.array(nums)  *#List converted into array*
**>>>** arr
array([[1, 2, 3], 
       [4, 5, 6]])
```

# 数组创建例程:

‣ **零** -返回一个用零填充的给定形状的数组

```
**>>>** arr = np.zeros(5)
**>>>** arr
array([0., 0., 0., 0., 0.])
**>>>** arr1 = np.zeros((2, 3))
**>>>** arr1
array([[0., 0., 0.], 
       [0., 0., 0.]])
```

‣**1**——返回一个给定形状的数组，用 1 填充

```
**>>>** arr = np.ones(5)
**>>>** arr
array([1., 1., 1., 1., 1.])
**>>>** arr = np.ones((3, 2))
array([[1., 1.], 
       [1., 1.], 
       [1., 1.]])
```

‣ **恒等式** -返回恒等式矩阵(即对角线上有 1 的数组)

```
**>>>** iden_mat = np.identity(3)
**>>>** iden_mat
array([[1., 0., 0.],
       [0., 1., 0.],
       [0., 0., 1.]])
```

‣ **eye** -返回对角线上有 1 而其他地方有 0 的二维数组

```
**>>>** arr = np.eye(4)
**>>>** arr
array([[1., 0., 0., 0.],        
       [0., 1., 0., 0.],        
       [0., 0., 1., 0.],        
       [0., 0., 0., 1.]])
**>>>** arr = np.eye(3, 4)
**>>>** arr
array([[1., 0., 0., 0.],        
       [0., 1., 0., 0.],        
       [0., 0., 1., 0.]])
```

‣ **诊断** -构建一个对角数组

```
**>>>** arr = np.diag([1, 2, 3, 4])
**>>>** arr
array([[1, 0, 0, 0],
       [0, 2, 0, 0],
       [0, 0, 3, 0],
       [0, 0, 0, 4]])
```

‣ **arange** -返回指定间隔内均匀分布的值。

```
**>>>** arr = np.arange(3, 7)
**>>>** arr
array([3, 4, 5, 6])
```

‣ **linspace** -返回给定范围内均匀分布的数字。

```
**>>>** arr = np.linspace(1, 15, 10)
**>>>** arr
array([ 1\.        , 2.55555556, 4.11111111, 5.66666667, 7.22222222, 8.77777778, 10.33333333, 11.88888889, 13.44444444, 15\.        ])
```

# 数组操作例程:

![](img/36cc50f4cff33191f695c0950dc51a12.png)

Photo by [Bradyn Trollip](https://unsplash.com/@bradyn?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

‣ **形状** -返回数组的形状。

```
**>>>** arr = np.array([[2, 3], [4, 5]])
**>>>** arr
array([[2, 3],
       [4, 5]])
**>>>** arr.shape
(2, 2)
```

‣ **重塑**-**-**改变数组的形状而不改变其数据。

```
**>>>** arr = np.array([1, 2, 3, 4, 5, 6])
**>>>** arr.reshape(2, 3)
array([[1, 2, 3],
       [4, 5, 6]])
```

‣ **ravel -** 返回连续的扁平数组

```
**>>>** arr = np.array([[1, 2, 3], [4, 5, 6]])
**>>>** np.ravel(arr)
array([1, 2, 3, 4, 5, 6])
```

# 二元运算:

![](img/fff127cafd040f8024d97855c227a8b4.png)

Photo by [d kah](https://unsplash.com/@d_kah?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

‣ **按位与**——对两个数组元素进行按位与运算。

```
**>>>** arr = np.bitwise_and([10,7], [14,20])
**>>>** arr
array([10,  4], dtype=int32)
```

‣ **按位“或”**——对两个数组元素进行按位“或”运算。

```
**>>>** arr = np.bitwise_or([10,7], [14,20])
**>>>** arr
array([14, 23], dtype=int32)
```

‣ **bitwise_xor** -对两个数组元素进行逐位 xor 运算。

```
**>>>** arr = np.bitwise_xor([10,7], [14,20])
**>>>** arr
array([ 4, 19], dtype=int32)
```

# 数学函数:

![](img/da6c3a31aa5076ced9ad4eaae3cdb2ee.png)

Photo by [Jeswin Thomas](https://unsplash.com/@jeswinthomas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **1。三角函数**

‣ **正弦-** 三角正弦元素式。

```
**>>>** arr = np.array([2, 80, 1, 97])
**>>>** arr
array([ 2, 80,  1, 97])
**>>>** np.sin(arr)
array([ 0.90929743, -0.99388865,  0.84147098,  0.37960774])
```

‣ **cos -** 三角余弦元素式。

```
**>>>** arr = np.array([4, 8, 10, 90])
**>>>** arr
array([ 4,  8, 10, 90])
**>>>** np.cos(arr)
array([-0.65364362, -0.14550003, -0.83907153, -0.44807362])
```

‣ **tan -** 逐元素计算三角正切。

```
**>>>** arr = np.array([14, 1, 20, 9])
**>>>** arr
array([14,  1, 20,  9])
**>>>** np.tan(arr)
array([ 7.24460662,  1.55740772,  2.23716094, -0.45231566])
```

## 2.双曲函数

‣ **sinh -** 双曲正弦元素式。

```
**>>>** arr = np.array([2, 80, 1, 97])
**>>>** arr
array([ 2, 80,  1, 97])
**>>>** np.sinh(arr)
array([3.62686041e+00, 2.77031119e+34, 1.17520119e+00, 6.69167360e+41])
```

‣ **余弦-** 双曲余弦元素式。

```
**>>>** arr = np.array([4, 8, 10, 90])
**>>>** arr
array([ 4,  8, 10, 90])
**>>>** np.cosh(arr)
array([2.73082328e+01, 1.49047916e+03, 1.10132329e+04, 6.10201647e+38])
```

‣ **tanh -** 双曲正切元素式。

```
**>>>** arr = np.array([14, 1, 20, 9])
**>>>** arr
array([14,  1, 20,  9])
**>>>** np.tanh(arr)
array([1\.        , 0.76159416, 1\.        , 0.99999997])
```

## 3.杂项功能

考虑以下概念的数组`array([2, 80, 1, 97, 5])`

‣ **min -** 返回数组的最小元素。

```
**>>>** np.min(arr)
1
```

‣ **max -** 返回数组的最大元素。

```
**>>>** np.max(arr)
97
```

‣ **argmin -** 返回最小元素的索引。

```
**>>>** np.argmin(arr)
2
```

‣ **argmax -** 返回 max 元素的索引。

```
**>>>** np.argmax(arr)
3
```

‣返回一个数组的平方根。

```
**>>>** np.sqrt(arr)
array([1.41421356, 8.94427191, 1\.        , 9.8488578 , 2.23606798])
```

‣返回所有元素的指数。

```
**>>>** np.exp(arr)
array([7.38905610e+00, 5.54062238e+34, 2.71828183e+00, 1.33833472e+42, 1.48413159e+02])
```

# 线性代数:

![](img/828fd0fae9ffff15079fcaf5a026d698.png)

Photo by [Wim van 't Einde](https://unsplash.com/@wimvanteinde?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

‣ **rank -** 返回数组的秩。

```
**>>>** arr = np.arange(1, 10).reshape(3, 3)  *#consider this array*
**>>>** arr                                   *#for below concepts* 
array([[1, 2, 3], 
       [4, 5, 6],
       [7, 8, 9]])
**>>>** rank = np.linalg.matrix_rank(arr)
**>>>** rank
2
```

‣ **踪迹**t31】-返回一个数组的踪迹。

```
**>>>** trace = np.trace(arr)
**>>>** trace
11
```

计算一个数组的行列式。

```
**>>>** det = np.linalg.det(arr)
**>>>** det
-306.0
```

‣ **inv -** 计算矩阵的逆矩阵。

```
**>>>** inv = np.linalg.inv(arr)
**>>>** inv
array([[ 0.17647059, -0.00326797, -0.02287582],
       [ 0.05882353, -0.13071895,  0.08496732],
       [-0.11764706,  0.1503268 ,  0.05228758]])
```

‣ **幂-** 计算数组中所有元素的幂。

```
**>>>** mat_pow = np.linalg.matrix_power(arr, 3)
**>>>** mat_pow
array([[336, 162, 228],
       [406, 162, 469],
       [698, 702, 905]])
```

‣ **eigh -** 计算数组的特征值。

```
**>>>** arr = np.array([[1, -2j], [2j, 5]])
**>>>** arr
array([[ 1.+0.j, -0.-2.j],
       [ 0.+2.j,  5.+0.j]])
**>>>** eg1, eg2 = np.linalg.eigh(arr)
**>>>** eg1
array([0.17157288, 5.82842712])
**>>>** eg2
array([[-0.92387953+0.j, -0.38268343+0.j],
       [ 0.+0.38268343j,  0.-0.92387953j]])
```

‣**-**返回**-**两个数组的点积。

```
>>> vector_a = 2 + 3j
>>> vector_b = 4 + 5j
>>> product = np.dot(vector_a, vector_b)
>>> product
(-7+22j)
```

‣ **vdot -** 返回两个向量的点积。

```
>>> vector_a = 2 + 3j
>>> vector_b = 4 + 5j
>>> product = np.vdot(vector_a, vector_b)
>>> product
(23-2j)
```

# 结论

本文解释了什么是 Numpy 数组，如何使用它，以及它的各种功能和策略。我希望你喜欢我的工作。请随意分享、讨论和表达你的想法和意见。

![](img/fde29cdcea0015da29a90f35a52aca93.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)