# LeetCode —在旋转排序数组中查找最小值

> 原文：<https://medium.com/geekculture/leetcode-find-minimum-in-rotated-sorted-array-eeccc3db79ca?source=collection_archive---------9----------------------->

# 问题陈述

假设一个按升序排序的长度为 *n* 的数组被**旋转**1 到 n 次。例如，数组 *nums = [0，1，2，4，5，6，7]* 可能变成:

*【4，5，6，7，0，1，2】*如果旋转 *4* 次。*【0，1，2，4，5，6，7】*如果旋转 *7* 次。注意**旋转**一个数组[a[0]，a[1]，a[2]，…，a[n — 1]] 1 次得到数组[a[n — 1]，a[0]，a[1]，a[2]，…，a[n — 2]]。

给定**唯一**元素的排序旋转数组 *nums* ，返回该数组的最小元素*。*

你必须写一个运行时间为 O(log n)的算法。

问题陈述摘自:[https://leet code . com/problems/find-minimum-in-rotated-sorted-array/](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)。

**例 1:**

```
Input: nums = [3, 4, 5, 1, 2]
Output: 1
Explanation: The original array was [1, 2, 3, 4, 5] rotated 3 times.
```

**例二:**

```
Input: nums = [4, 5, 6, 7, 0, 1, 2]
Output: 0
Explanation: The original array was [0, 1, 2, 4, 5, 6, 7] and it was rotated 4 times.
```

**例 3:**

```
Input: nums = [11, 13, 15, 17]
Output: 11
Explanation: The original array was [11, 13, 15, 17] and it was rotated 4 times.
```

**约束:**

```
- n == nums.length
- 1 <= n <= 5000
- -5000 <= nums[i] <= 5000
- All the integers of nums are unique
- nums is sorted and rotated between 1 and n times
```

# 说明

## 强力方法

最简单的方法是进行线性搜索，这需要花费 **O(N)** 时间。我们需要找到第个索引的**，其中一个较小的数字出现在第**(I-1)个**个索引之后。**

## 二进位检索

由于数组是旋转但排序的，我们可以修改我们的二分搜索法实现。旋转后的数组有两个已排序的段。排序被打乱的索引出现在数组中的最小元素处。

让我们检查算法:

```
- set low = 0
      high = nums.size() - 1

- loop while low < high
  - set mid = low + (high - low) / 2

  - if nums[low] <= nums[mid] && nums[high] >= nums[mid]
    - set high = mid - 1
  - else if nums[low] <= nums[mid] && nums[high] <= nums[mid]
    - set low = mid + 1
  - else if nums[mid] <= nums[low]
    - set high = mid

- return nums[low]
```

让我们看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {
            int mid = low + (high - low) / 2;

            if(nums[low] <= nums[mid] && nums[high] >= nums[mid])
                high = mid - 1;
            else if(nums[low] <= nums[mid] && nums[high] <= nums[mid])
                low = mid + 1;
            else if(nums[mid] <= nums[low])
                high = mid;
        }

        return nums[low];
    }
};
```

## 戈朗溶液

```
func findMin(nums []int) int {
    low, mid := 0, 0
    high := len(nums) - 1

    for low < high {
        mid = low + (high - low) / 2

        if nums[low] <= nums[mid] && nums[high] >= nums[mid] {
            high = mid - 1
        } else if nums[low] <= nums[mid] && nums[high] <= nums[mid] {
            low = mid + 1
        } else if nums[mid] <= nums[low] {
            high = mid
        }
    }

    return nums[low]
}
```

## Javascript 解决方案

```
var findMin = function(nums) {
    let low = 0;
    let high = nums.length - 1;

    while(low < high) {
        let mid = low + Math.floor((high - low) / 2);

        if(nums[low] <= nums[mid] && nums[high] >= nums[mid])
            high = mid - 1;
        else if(nums[low] <= nums[mid] && nums[high] <= nums[mid])
            low = mid + 1;
        else if(nums[mid] <= nums[low])
            high = mid;
    }

    return nums[low];
};
```

让我们试运行一下我们的算法。

```
Input: nums = [4, 5, 6, 7, 0, 1, 2]

Step 1: low = 0
        high = nums.size() - 1
             = 7 - 1
             = 6

Step 2: loop while low < high
          0 < 6
          true

          mid = low + (high - low) / 2
              = 0 + (6 - 0) / 2
              = 3

          if nums[low] <= nums[mid] && nums[high] >= nums[mid]
             nums[0] <= nums[3] && nums[6] >= nums[3]
             4 <= 7 && 2 >= 7
             false

          else if nums[low] <= nums[mid] && nums[high] >= nums[mid]
                  nums[0] <= nums[3] && nums[6] <= nums[3]
                  4 <= 7 && 2 <= 7
                  true

                  low = mid + 1
                      = 3 + 1
                      = 4

Step 3: loop while low < high
          4 < 6
          true

          mid = low + (high - low) / 2
              = 4 + (6 - 4) / 2
              = 4 + 1
              = 5

          if nums[low] <= nums[mid] && nums[high] >= nums[mid]
             nums[4] <= nums[5] && nums[6] >= nums[5]
             0 <= 1 && 2 >= 1
             true

             high = mid - 1
                  = 5 - 1
                  = 4

Step 4: loop while low < high
          4 < 4
          false

Step 5: return nums[low]
               nums[4]
               0

We return the answer as 0.
```

*原发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-find-minimum-in-rotated-sorted-array)*。*