# 集合中的科特林特殊函数

> 原文：<https://medium.com/geekculture/kotlin-special-functions-in-collections-9f91bbd9dca6?source=collection_archive---------29----------------------->

![](img/f61f3d268a5c6795e86100c5cca3afa8.png)

Photo by [Jack Sloop](https://unsplash.com/@jacksloop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

上周，我浏览了我的一些旧项目，无意中发现了一些非常丑陋的旧代码。当我刚开始使用 Kotlin 编程语言时，我写了这段代码。在这段时间里，我并不知道所有的 Kotlin 特殊函数，而是自己编写代码，代码变得非常快，非常混乱。在这篇文章中，我想向你展示一些最重要和最酷的 Kotlin 特殊函数。

# 集合函数

## 1.创建新收藏

这些函数都用于实例化不同类型的集合。

```
// Empty Collection
[**emptyList**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/empty-list.html)**,** [**emptyMap**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/empty-map.html)**,** [**emptySet**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/empty-set.html)// Read-only Collection
[**listOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/list-of.html)**,** [**mapOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-of.html)**,** [**setOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/set-of.html)// Mutable Collection
[**mutableListOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-list-of.html)**,** [**mutableMapOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-map-of.html)**,** [**mutableSetOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-set-of.html)// Build Collection from mix sources
[**buildList**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/build-list.html)**,** [**buildMap**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/build-map.html)**,** [**buildSet**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/build-set.html)// Sorted Collection
[**sortedMapOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-map-of.html)**,** [**sortedSetOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-set-of.html)(more in [stackOverflow](https://stackoverflow.com/questions/61965983/whats-the-different-between-linkedsetof-and-hashsetof))// Hash Collection
[**hashMapOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/hash-map-of.html)**,** [**hashSetOf**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/hash-set-of.html)(more in [stackOverflow](https://stackoverflow.com/questions/61965983/whats-the-different-between-linkedsetof-and-hashsetof))
```

以下是这些不同类型的集合的一些实际例子。

## 2.转换为另一种类型的收藏

这些函数将帮助您将现有的集合转换为不同的类型。

```
// to array type
[**toBooleanArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-boolean-array.html)**,** [**toByteArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-byte-array.html)**,** [**toCharArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-char-array.html)**,** [**toDoubleArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-double-array.html)**,** [**toFloatArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-float-array.html)**,** [**toIntArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-int-array.html)**,** [**toLongArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-long-array.html)**,** [**toShortArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-short-array.html)**,** [**toTypedArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-typed-array.html)**,** [**toUByteArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-u-byte-array.html)**,** [**toUIntArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-u-int-array.html)**,** [**toULongArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-u-long-array.html)**,** [**toUShortArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-u-short-array.html)// to read-only collection
[**toList**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-list.html)**,** [**toMap**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-map.html)**,** [**toSet**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-set.html)// to mutable collection
[**toMutableList**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-mutable-list.html)**,** [**toMutableMap**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-mutable-map.html)**,** [**toMutableSet**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-mutable-set.html)**,** [**toHashSet**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-hash-set.html)// to sorted collection
[**toSortedMap**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-sorted-map.html)**,** [**toSortedSet**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-sorted-set.html)
```

这些函数非常方便，因为现在您不必创建一个新的可变映射，将旧映射的内容复制到其中，然后删除旧映射。它节省了大量时间，并使代码更具可读性。

这里还有一些你在现实生活中会用到的实际例子。

如您所见，您可以保留您的内容，并且仍然可以更改您的收藏类型。

## 3.更改收藏的内容

以下函数可用于更改集合本身的内容。

**添加和删除内容**

```
// Changing content
[***set***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/set.html)**,** [***setValue***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/set-value.html) //(more info in [stackoverflow](https://stackoverflow.com/questions/61912898/mutablemap-setvalue-function-no-longer-exist/61981005#61981005))// Adding content
[**plus**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/plus.html)**,** [**plusElement**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/plus-element.html), //(more info in [stackoverflow](https://stackoverflow.com/questions/61937712/whats-the-different-use-case-of-minus-vs-minuselement))
[***plusAssign***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/plus-assign.html)**,** //(more info in [stackoverflow](https://stackoverflow.com/questions/61937455/whats-the-different-between-plusassign-and-add-for-mutablelist-or-put-fo))
[***add***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/add.html)**,** [***addAll***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/add-all.html)**,** [***put***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-map/put.html)**,** [***putAll***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/put-all.html)// Removing content
[**minus**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/minus.html)**,** [**minusElement**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/minus-element.html)**,** //(more info in [stackoverflow](https://stackoverflow.com/questions/61937712/whats-the-different-use-case-of-minus-vs-minuselement))
[***minusAssign***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/minus-assign.html)**,** //(more info in [stackoverflow](https://stackoverflow.com/questions/61937455/whats-the-different-between-plusassign-and-add-for-mutablelist-or-put-fo))
[***remove***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html)// Remove away from the front or behind of the collection [**drop**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/drop.html)**,** [**dropLast**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/drop-last.html)**,** [**dropLastWhile**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/drop-last-while.html)**,** [**dropWhile**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/drop-while.html)**,** [***removeFirst***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove-first.html)**,** [***removeFirstOrNull***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove-first-or-null.html)**,** [***removeLast***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove-last.html)**,** [***removeLastOrNull***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove-last-or-null.html), [**take**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/take.html)**,** [**takeLastWhile**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/take-last-while.html)**,** [**takeLast**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/take-last.html)**,** [**takeWhile**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/take-while.html)// Pick from the center section of the collection
[**slice**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/slice.html)**,** [**sliceArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/slice-array.html)// Get unique values only
[**distinct**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/distinct.html)**,** [**distinctBy**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/distinct-by.html)
```

这里有一些关于如何使用它们的实际例子。

如您所见，除了普通的方法之外，还有更多方法可以改变可变集合。这没有问题，因为您可以毫不费力地将这两种类型相互转换。

**将内容转换为另一个值**

```
// Transform the content to another value
[**map**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map.html)**,** [**mapTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-to.html)**,** [**mapIndexed**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-indexed.html)**,** [**mapIndexedTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-indexed-to.html)**,** [**mapKeys**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-keys.html)**,** [**mapKeysTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-keys-to.html)**,** [**mapValues**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-values.html)**,** [**mapValuesTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-values-to.html)**,** [***replaceAll***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/replace-all.html)**,** [***fill***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/fill.html)// Transform the content and remove all null
// to have non-nullable result of content
[**mapNotNull**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-not-null.html)**,** [**mapNotNullTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-not-null-to.html)**,** [**mapIndexedNotNull**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-indexed-not-null.html)**,** [**mapIndexedNotNullTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-indexed-not-null-to.html)
```

地图是最重要的操作之一。Map 有机会返回集合数据类型之外的另一个数据类型。Map 主要用于直接在集合中计算东西，并将结果保存在这个集合中。

**过滤内容**

```
// Filter some content away
[**filter**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html)**,** [**filterIndexed**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-indexed.html)**,** [**filterIndexedTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-indexed-to.html)**,** [**filterIsInstance**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-is-instance.html)**,** [**filterIsInstanceTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-is-instance-to.html)**,** [**filterKeys**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-keys.html)**,** [**filterNot**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-not.html)**,** [**filterNotNull**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-not-null.html)**,** [**filterNotNullTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-not-null-to.html)**,** [**filterNotTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-not-to.html)**,** [**filterTo**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-to.html)**,** [**filterValues**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter-values.html)
```

这些函数将从集合中过滤掉一些东西，只返回满足过滤标准的值。过滤函数的工作方式与贴图函数非常相似。您可以定义要用于“it”值的参数，也可以保留默认值。

**整理收藏**

```
// Sort the content value
[**sorted**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted.html)**,** [**sortedArray**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-array.html)**,** [**sortedArrayDescending**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-array-descending.html)**,** [**sortedArrayWith**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-array-with.html)**,** [**sortedBy**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-by.html)**,** [**sortedByDescending**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-by-descending.html)**,** [**sortedDescending**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-descending.html)**,** [**sortedWith**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sorted-with.html)[***sort***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sort.html)**,** [***sortBy***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sort-by.html)**,** [***sortByDescending***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sort-by-descending.html)**,** [***sortDescending***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sort-descending.html)**,** [***sortWith***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sort-with.html)// Randomize the content sequence
[***shuffle***](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/shuffle.html)**,** [**shuffled**](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/shuffled.html)
```

这些函数应该是不言自明的。它们可以帮助你将你的收藏按照特定的或者不太特定的顺序排序。

# 反射

## 什么进展顺利

我认为通过写下 Kotlin collections 的所有这些特殊功能，我学到了很多东西。我确信这将在未来对我有所帮助，并使我的代码更容易理解和维护。

## 什么需要改进

在我开始自己的项目之前，我应该做更多的练习，这样我会更好地了解这些特殊功能。