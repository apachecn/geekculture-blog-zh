# 脸书采访:合并 k 排序列表

> 原文：<https://medium.com/geekculture/facebook-interview-merge-k-sorted-lists-5e028065156a?source=collection_archive---------13----------------------->

给你一个`k`链表`lists`的数组，每个链表按升序排序。

*将所有链表合并成一个排序后的链表并返回。*

**例一:**

```
**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**例 2:**

```
**Input:** lists = []
**Output:** []
```