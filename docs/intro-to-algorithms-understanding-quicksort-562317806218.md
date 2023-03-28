# 算法简介:了解快速排序

> 原文：<https://medium.com/geekculture/intro-to-algorithms-understanding-quicksort-562317806218?source=collection_archive---------22----------------------->

## 分而治之策略

## 解释了一个基本算法

![](img/84f5c4796d48eff81d7042b7c311c588.png)

Photo by [David Bruno Silva](https://unsplash.com/@brlimaproj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/files?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[**快速排序**](https://en.wikipedia.org/wiki/Quicksort) 算法是利用 [*分治范例*](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm) 的众多例子之一。顾名思义，这种范式包括将问题分解成越来越小的块，直到达到一个[基础案例](https://en.wikipedia.org/wiki/Recursion#base_case)。在这一点上，你已经把问题分解成足够小的部分，对于存在的少数基本情况来说更容易解决。在我们研究如何实现快速排序算法时，请记住这一策略。

# 分割:分割问题

简单地说，QuickSort 的工作原理是选择一个 **pivot 元素**，然后使用它对数组中的剩余元素进行排序。值小于或等于 pivot 的任何元素被移动到数组中 pivot 的“*左*”，而大于 pivot 的任何元素被移动到“*右*”。随着父数组**被分割**(或者 ***将*** 一分为二)，我们接着使用相同的方法对子数组进行排序，不断分割数组，直到所有元素都被排序。

让我们来看看 Javascript 是什么样子的:

Syntactically clear quickSort function. It does not sort “[in place](https://en.wikipedia.org/wiki/In-place_algorithm)” and is therefore not the most memory-efficient implementation.

分区逻辑从第 17 行开始，将小于或等于 pivot 的所有元素推入`front`子数组，将大于 pivot 的所有元素推入`back`子数组。这些元素相对于枢轴被*分开*，但是按照它们被处理的顺序被放置到它们相应的子数组中。这些子数组没有被排序，但是我们已经成功地将问题一分为二，并且可以继续这样做，直到我们达到我们的基本情况。

# 征服基础案例

快速排序的基本情况非常简单。**如果当前数组的元素少于两个(即，它要么只有一个元素，要么为空),则按原样排序并返回。**每个子数组被重复分割，递归分割，直到达到基本情况，此时各个子数组被连接在一起以返回排序的父数组。

虽然上面的代码示例实现了*和*的功能，但这并不是你在技术面试中想要写出来的东西。*一个合适的快速排序算法应该将* [***就地***](https://en.wikipedia.org/wiki/In-place_algorithm) 排序，对提供的数组进行排序而不使用额外的数据结构。上面的实现实际上返回了一个新的排序后的数组，同时保持原来的数组不变。

***暂且不说*** *: QuickSort 的速度部分源于它对集合进行就地排序的能力。MergeSort 和 QuickSort 的平均运行时间都是 O(n log(n))，但是 QuickSort 就地排序，而 MergeSort 必须分配额外的内存。分配和释放内存会给 MergeSort 的运行时间增加一个常数。在分析算法运行时，常量通常会被忽略。但是，由于 QuickSort 和 MergeSort 具有相同的时间复杂度，内存分配带来的额外常数使 MergeSort 成为较慢的算法。* [*点击此处了解更多关于合并排序与快速排序的对比*](https://www.geeksforgeeks.org/quick-sort-vs-merge-sort/)

# **分区就位**

有两种流行的算法用于就地划分数组。Lamuto 和 Hoare 分区方案都成功地对给定的数组进行了排序，尽管 Hoare 方案通常被认为更有效。Lamuto 的方案平均执行 3 倍于 Hoare 的方案。当数组包含许多重复的元素时，霍尔的方案胜出，因为 Lamuto 的方案会不必要地重复交换重复的元素。

# 拉穆托的计划

Visual demonstration of Lamuto’s scheme. As mentioned in the video description, it is more efficient to swap the pivot with the element at array[i + 1] than to shift all the elements down.

传统上，Lamuto 的分区方案使用数组中的最后一个元素作为支点。然后，两个索引`i`和`j`遍历数组。`i`跟踪最后一个小于或等于主元的元素。当`array[j]`小于或等于支点时，`i`递增，然后`array[i]`和`array[j]`交换。

Javascript implementation of the Lamuto partition scheme. Note: there are minor differences between the code shown above and the visualization, but the effect is the same.

## 霍尔方案

Visualization via [HackerEarth](https://www.hackerearth.com/practice/algorithms/sorting/quick-sort/visualize/)

霍尔的方案也使用了两个指针，尽管方式不同。指针逐渐收敛，搜索相对于其位置而言过大或过小的元素。交换这两个元素，指针继续收敛，直到它们相互交叉。当指针交叉时，所有元素都被排序到数组的正确一侧，我们用第一个已知大于或等于枢轴值的元素交换枢轴。返回分区索引(即中枢的最终排序位置)，以便可以递归排序剩余的子数组。

# 不足之处

如前所述， **QuickSort 通常比 MergeSort 快，尽管有些情况下 MergeSort 更好**。因为 QuickSort 依赖于对数据集合的*随机访问*(不按顺序读取集合中的元素)**，所以在对存储在外部存储上的大型数据集进行排序时效率较低**。MergeSort 依赖于对内存的顺序访问，这使它在处理外部存储时具有优势。外部存储通常保存在硬盘上，[随机访问数据的成本高于顺序访问](https://www.youtube.com/watch?v=IvVZ7jf8wqw)。

同样的， **QuickSort 在处理链表**时也并不出色。同样，因为链表天生需要顺序访问，所以快速排序使用的随机访问不是最佳的。在这些情况下，MergeSort 的性能更好。

**QuickSort 在处理所有元素都有相同值的数组时性能很差**。由于算法的性质，具有相同值的元素在划分过程中不必要地交换，导致时间复杂度为 O(n)。

**如果没有选择随机枢纽——而是选择总是选择数组中的第一个(或最后一个)元素——那么当所提供的数组已经排序时，二次时间复杂度可靠地发生**。我们的分区方法——不考虑所选的方案——返回枢轴的最终排序索引，该索引用于划分父数组并对其进行递归排序。如果第一个元素总是我们的枢纽，并且父数组已经排序，那么分区过程每次只删除一个元素。我们最终遍历每个 pivot 元素的所有元素，导致 O(n)时间复杂度。

# 结论

QuickSort 将提供的数据集划分为更小的块，快速降低了问题的复杂性，并允许相对快速的排序算法。在这篇文章中多次提到的 MergeSort，也利用了分而治之的策略(也许将是下一篇文章的主题？).QuickSort 擅长数组和较小的数据集，但在链表、较大的数据集和有多个重复值的情况下却很吃力。

```
**References and Further Reading**[QuickSort Algorithm — Abdul Bari](https://www.youtube.com/watch?v=7h1s2SojIRw)[QuickSort — GeeksForGeeks](https://www.geeksforgeeks.org/quick-sort/)[Hoare's vs Lomuto Partition Scheme in QuickSort — GeeksForGeeks](https://www.geeksforgeeks.org/hoares-vs-lomuto-partition-scheme-quicksort/)[Quick Sort(Hoare’s Partition) Visualization — GeeksForGeeks](http://Quick Sort(Hoare’s Partition) Visualization using JavaScript)[Quick Sort vs Merge Sort — GeeksForGeeks](https://www.geeksforgeeks.org/quick-sort-vs-merge-sort/)[Grokking Algorithms: An Illustrated Guide for Programmers and Other Curious People](https://bookshop.org/books/grokking-algorithms-an-illustrated-guide-for-programmers-and-other-curious-people/9781617292231)
```