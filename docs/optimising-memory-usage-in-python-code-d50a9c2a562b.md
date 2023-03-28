# 优化 Python 代码中的内存使用

> 原文：<https://medium.com/geekculture/optimising-memory-usage-in-python-code-d50a9c2a562b?source=collection_archive---------14----------------------->

在本文中，我们将讨论一些有用的资源/技巧来优化 python 中的内存使用，让我们直接切入主题。

![](img/bd0a1ce2c592a9e4fdb6439871af2954.png)

Photo by [J. Kelly Brito](https://unsplash.com/@hellokellybrito?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 缓存它

数据密集型项目很难避免下载大量数据，如网站和数据库转储、推文、其他社交媒体帖子等。这些下载需要大量时间，消耗带宽和我们的信任度。如果数据源对我们的交易不是特别感兴趣，它可能会禁止或限制我们的 IP 地址，使进一步的下载变得缓慢或不可能。因此，引用一个相对用户友好的网站，礼貌的数据矿工缓存在他们的结束；[无礼者被禁](https://dev.livejournal.com/653177.html)。一般来说，我们希望缓存我们下载的任何东西，除非我们有充分的理由相信我们不再需要它，或者它会在我们再次需要它之前过期。

*   一种经典的缓存方法是通过标识符组织一个目录来存储以前获得的对象。例如，标识符可以是对象的 URL、tweet ids 或数据库行号；任何与物品来源相关的东西。
*   下一步是将标识符转换成外观一致的唯一文件名。我们可以自己编写转换函数，也可以使用标准库。首先对标识符进行编码，它可能是一个字符串。应用一个散列函数，比如`hashlib.md5()`或者更快的`hashlib.sha256()`，来获得一个散列对象。这些函数不会产生完全唯一的文件名，但是获得两个相同文件名的可能性(称为**哈希冲突**)非常低，我们可以忽略它。最后，获取对象的十六进制摘要。摘要只是一个 64 个字符的 ASCII 字符串:一个完美的文件名，与原始对象标识符没有任何相似之处。

```
import hashlib
source = ‘https://lj-dev.livejournal.com/653177.html'
hash = hashlib.sha256(source.encode())
filename = hash.hexdigest()
print(hash, filename)
```

假设已经创建了目录缓存，并且它是可写的，那么我们可以将我们的对象放入其中。

首先，检查对象是否已经被酸洗。如果有，我们已经下载了它，没有必要再从源代码中获取它。

但首先，检查对象是否已经被腌制。如果是的话，我们已经下载了。不需要再次从源获取它。

```
cache = f'cache/{filename}.p' 
try:
  with open(cache, 'rb') as infile:
      # Has been pickled before! Simply unpickle 
      object = pickle.load(infile)
except FileNotFoundError:
    # Download and pickle
    object = 'https://lj-dev.livejournal.com/653177.html' 
    with open(cache, 'wb') as outfile:
      pickle.dump(outfile, object) 
except:
    # Things happen...
```

我们可能希望将前面提到的片段组合成一个函数，该函数接受一个对象标识符，并从源或本地缓存中检索对象。缓存是相当令人兴奋的行为；对函数的第一次调用可能比连续调用花费更多的时间。

# 就地大排序

排序和搜索可以说是现代计算中最频繁和最重要的两种操作。它们如此频繁，以至于全球市值第二大的科技公司 Alphabet(谷歌)靠 n 排序和搜索发了财。它们如此重要，以至于 Python 有两个函数用于对列表进行排序:`list.sort()`和`sorted()`。

## 有什么区别？

*   首先，`sorted()`对任何可迭代的进行排序，而`list.sort()`只对列表进行排序。鉴于 Python 对列表的重视，这似乎不是问题。
*   其次，`sorted()`创建原始 iterable 的排序副本。当它返回时，我们得到两个 iterables:原始的和排序的，这是好的和坏的。这很糟糕，因为我们需要两倍的内存。这对于一个小列表来说没什么大不了的，但是对于一个大数据集来说却是一个重要的问题。当我们有一个十亿条的列表时，我们可能会想是否要复制它。作为一种补偿，我们仍然可以按照原来的顺序访问列表项。

`list.sort()`方法对列表进行排序。它打乱了列表项的顺序，但没有制作副本。如果我们能够将列表加载到内存中，我们肯定能够对其进行排序。然而，`list.sort()`却破坏了原有的秩序。有趣的是，这两种排序工具的性能大致相同。

**总结一下，如果我们的列表很大，用** `**list.sort()**` **排序到位。如果我们的列表大小适中或者需要保持原来的顺序，调用** `**sorted()**` **并检索一个排序后的副本。**

# 删除你的垃圾

Python 是一种具有隐式内存管理的语言。C 和 C++语言要求我们自己分配和释放内存。Python 自己管理分配和释放。当我们通过赋值语句定义一个变量时，Python 会创建变量和与之相关的对象。

## 何时销毁

每个 Python 对象都有一个**引用计数**，即引用该对象的变量和其他对象的数量。当我们创建一个对象，并且没有把它赋给一个变量时，这个对象就没有引用:

```
'Hello, world!' # An object without references
```

如果一个对象没有被引用，我们就不能使用它。特别是，我们不能把上一个例子中的字符串转换成小写。但是，如果我们将字符串赋给一个变量，我们将引用它并在将来使用它:

```
s = 'Hello, world!' # An object with single reference
```

我们可以通过将同一个对象赋给几个变量来创建更多的引用:

```
s3 = s2 = s1 = s # Four references to the same object!
```

当我们重新定义一个变量时，它不再指向旧的对象，引用计数减少:

```
s = 'Goodbye, world!' # Only three references remain
```

参考可以是间接的。例如，strList 列表包含对原始字符串的引用。如果我们进一步重新定义 s1、s2 和 s3，只要列表被引用，字符串的引用计数仍然是正的:

```
strList = [s1]
s1 = s2 = s3 = None
```

## 垃圾收集工

当引用计数变为零时，对象变得不可达。出于任何实际目的，一个不可及的对象就是一堆垃圾。称为**垃圾收集器**的 Python 运行时的一部分自动收集并丢弃未被引用的对象。很少有必要去干扰它，但是这里有一个场景，这种干扰是有帮助的。

假设我们使用*大数据*——不是大数据，而是大到足以给我们的计算机 RAM 带来压力的数据。我们从原始数据集开始，逐步对其应用昂贵的转换，并记录中间结果。一个中间结果可以用于一个以上的后续转换。最终，我们的计算机内存将被大对象堵塞，其中一些仍然需要，而一些则不需要。我们可以通过使用`del`操作符显式标记要删除的变量和相关对象来帮助 Python:

```
bigData = ...
bigData1 = func1(bigData) 
bigData2 = func2(bigData)
del bigData # Not needed anymore
```

记住`del`不会从内存中移除对象。它只是将其标记为未引用，并销毁其标识符。垃圾收集器仍然必须干预并收集垃圾。我们可能希望通过模块`gc`中提供的接口，在预计到大量内存使用的情况下，立即强制进行垃圾收集

```
import gc # Garbage Collector
gc.collect()
```

**不要滥用这个功能。垃圾收集需要很长时间，所以我们应该只让它在必要的时候发生。**

今天就到这里，希望您已经收集了一些关于优化 python 内存的信息。下次见，再见