# 简化的两个排序数组的中间值

> 原文：<https://medium.com/geekculture/median-of-two-sorted-arrays-simplified-c128d26dbd75?source=collection_archive---------24----------------------->

## 易于理解的 LeetCode 难题:两个有序数组的中值

在 LeetCode 中，寻找两个有序数组的中间值是一个困难的问题。这是一个常见的 FAANG 面试问题。网上有几个解决方案，但是很难理解。这里将对其进行简化，便于理解其核心。

![](img/b7599edba0297d6d26e11a8700744f01.png)

Photo by [Edoardo Busti](https://unsplash.com/@phoedobus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 问题

给定两个大小分别为`m`和`n`的排序数组`nums1`和`nums2`，返回两个排序数组的中间值**。**

总的运行时间复杂度应该是`O(log (m+n))`。

LeetCode 最初的问题在

[https://leetcode.com/problems/median-of-two-sorted-arrays](https://leetcode.com/problems/median-of-two-sorted-arrays)

## 解决办法

有一些快速且易于理解的解决方案:

**方案一**:合并，求中位数。由于两者都是排序数组，时间复杂度为 O(m+n)。

**方案二**:合并到中间。因为它们是排序的，所以我们不需要合并所有，而是在找到中间值后返回。所以时间复杂度是 O((m+n)/2)。

这些都很好理解但是没有优化到 O(log(m+n))。

**解决方案 3**:leet code 中提出了一个解决方案。但是很难理解。我们可以把问题转化成一个著名的解:divide&convert(D&C)。现在可以简化为以下 3 个步骤:

1.  D&C 方法:在一个较短的阵列中拾取一个介质，然后通过在两端均匀计数得到另一个阵列中的介质。
2.  检查两种介质是否相同。如果相同，返回结果。
3.  否则，更新新的介质索引。

关于如何更新介质索引的细节可以参考 LeetCode 的[解决方案](https://leetcode.com/problems/median-of-two-sorted-arrays/solutions/2471/very-concise-o-log-min-m-n-iterative-solution-with-detailed-explanation/)的解释。也就是说，利用 D & C 方法的详细指数发现。

现在我们可以有一个如下的 Java 解决方案:

```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1 == null || nums2 == null)
            return -1;

        // Keep nums2 shorter
        if (nums1.length < nums2.length) 
            return findMedianSortedArrays(nums2, nums1);

        int n1 = nums1.length;
        int n2 = nums2.length;
        int lo = 0, hi = n2 * 2;
        while (lo <= hi) {
            int mid2 = (lo + hi) / 2;   // D&C pick up in the shorter one
            int mid1 = n1 + n2 - mid2;  // Calculate based on mid2

            double L1 = (mid1 == 0) ? Integer.MIN_VALUE : nums1[(mid1-1)/2]; // Get L1, R1, L2, R2 respectively
            double L2 = (mid2 == 0) ? Integer.MIN_VALUE : nums2[(mid2-1)/2];
            double R1 = (mid1 == n1 * 2) ? Integer.MAX_VALUE : nums1[(mid1)/2];
            double R2 = (mid2 == n2 * 2) ? Integer.MAX_VALUE : nums2[(mid2)/2];

            if (L1 > R2) 
                lo = mid2 + 1; // nums1 lower half is too big; move up
            else if (L2 > R1) 
                hi = mid2 - 1; // nums2 lower half is too big; move down
            else 
                return (Math.max(L1,L2) + Math.min(R1, R2)) / 2;    // Find the medium
        }

        return -1;  
    }
}
```

它可以通过 100%的 LeetCode 测试用例。其时间复杂度为 O(log(min(m，n)))。

简单地说，请考虑这是一个求 O(logn)解的 D&C 问题。然后首先在较短的数组中除法，在另一个数组中寻找索引。不断迭代。

编码快乐！

*问题，想法？在这里留下评论。跟随我成为有趣的解决问题之旅的一部分。*