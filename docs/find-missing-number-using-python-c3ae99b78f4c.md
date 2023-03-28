# 查找缺失的数字:使用 Python

> 原文：<https://medium.com/geekculture/find-missing-number-using-python-c3ae99b78f4c?source=collection_archive---------6----------------------->

## 让我们利用这个介绍性的问题来发展算法思维

![](img/cc54231108e114aef6ecaafa99d5160e.png)

[https://github.com/keon/algorithms](https://github.com/keon/algorithms)

这是一个简单的水平问题，但我们试图用多种方法来解决它。这个例子是为那些试图提高解决问题能力的初学者准备的。

**问题陈述:**

> 给定一个包含范围`*[0, n]*`中的`*n*`个不同数字的数组`*nums*`，返回数组中唯一缺少的数字*。*
> 
> 测试用例示例:
> **输入:** nums = [3，0，1]
> **输出:** 2
> **输入:** nums = [0，1]
> **输出:** 2
> **输入:** nums = [0]
> **输出:** 1

在开始实现之前，我们应该通过理解问题陈述来发展我们的直觉。

假设我们有一个大小为 6(数组长度)的数组，由[3，2，1，0，6，5]组成。任务是从给定的数组中找到缺失的值。一看就可以说少了 4，对。我们将开发一种算法来找到这个丢失的数字。

**方法#1:** [**蛮力**](https://www.freecodecamp.org/news/brute-force-algorithms-explained/)
算法:
1。在数组长度范围内线性扫描数组(含)
2。检查迭代值是否在数组
3 中。如果不在数组中，则返回值

```
def missingNumber(self, nums):
    n = len(nums)
        for num in range(length+1):#traversing through range of n
            if num not in nums:   #searching for the number in array
                return num        #returning the missing number
```

时间复杂度:O(n )
空间复杂度:O(1)

在数组中搜索的时间复杂度是 O(n ),并且因为它是在线性扫描的循环中，所以时间复杂度是 O(n)。因此，总时间复杂度是 n*n 或 n，即，对于每次迭代，它查看 n 个项目(在最坏的情况下)。由于除了长度(取 O(1))之外，我们没有使用任何空间来存储，所以空间复杂度保持不变，为 O(1)。

**方法#2:修改蛮力使用** [**设置**](https://docs.python.org/3/tutorial/datastructures.html#sets)
算法同上，略有修改。我们使用 *set* 来存储数组值，然后搜索帮助我们大幅提高性能的元素。
算法:1。扫描通过 ***设置*** 在数组长度范围内线性(含)
2。检查迭代值是否在集合
3 中。如果不在集合中，则返回值

```
def missingNumber(self, nums):
        nums_set = set(nums)    #inserting array elements into a set
        length = len(nums)
        for num in range(length+1):#traversing through the array
            if num not in nums_set: #searching for the number in set
                return num
```

时间复杂度:O(n)
空间复杂度:O(n)

这就有意思了！仅仅通过实现 set，我们就能够将时间复杂度从 O(n)降低到 O(n)，**怎么做？**
—在一个集合中搜索的时间复杂度平均为 O(1)，其中 as for 和 since 是线性扫描的循环，时间复杂度为 O(n)。所以总的时间复杂度是 O(n)。
由于我们创建了一个长度等于 array 的集合，使用的空间大小为 n，所以空间复杂度为 O(n)。

在这种方法中，我们能够以空间复杂度为代价降低时间复杂度。这叫做*取舍。*

**方法#3:高斯公式** 你应该听说过 n 个自然数之和即 n*(n+1)/2 的公式。这就是著名的高斯公式。我们将使用这种技术来解决我们的问题。

算法:
1。计算 n(给定数组的长度)个数的和。设这个为 sum_n
2。计算数组中元素的和，设这个为 sum_ele
3。返回 sum_n 和 sum_ele 之差

```
def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        sum_of_n = ((n*(n+1))/2)  #sum of n numbers
        sum_of_elems = 0
        for num in nums:
            sum_of_elems += num    #total sum of elements in array

        return int(sum_of_n - sum_of_elems) #return missing number
```

时间复杂度:O(n)
空间复杂度:O(1)

我们线性遍历数组，即 O(n)时间复杂度。
我们还没用过

这看起来是一个最佳解决方案。但这不能应用于长的长数字，因为 n*(n+1)的计算导致一个 [*整数溢出*](https://en.wikipedia.org/wiki/Integer_overflow) *。*

我们可以使用位操作来解决这个约束。

**方法#4:使用位操作**

这需要对[位操作](https://www.hackerearth.com/practice/basic-programming/bit-manipulation/basics-of-bit-manipulation/tutorial/)有一点了解，算法和上面一样简单。这比上面的方法效率高得多。

```
def missingNumber(self, nums):
        missing_num = len(nums)
        for i, num in enumerate(nums):
            missing_num ^= i^ num
        return missing_num
```

时间复杂度:O(n)
空间复杂度:O(1)

随意评论。请补充/建议其他方法。