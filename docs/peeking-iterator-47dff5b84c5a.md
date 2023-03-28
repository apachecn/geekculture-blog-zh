# 窥视迭代器

> 原文：<https://medium.com/geekculture/peeking-iterator-47dff5b84c5a?source=collection_archive---------14----------------------->

你能解决这个真正的谷歌面试问题吗？

设计一个迭代器，除了支持`hasNext`和`next`操作之外，还支持现有迭代器上的`peek`操作。

实现`PeekingIterator`类:

*   `PeekingIterator(Iterator<int> nums)`用给定的整数迭代器`iterator`初始化对象。
*   `int next()`返回数组中的下一个元素，并将指针移动到下一个元素。
*   `boolean hasNext()`如果数组中还有元素，返回`true`。