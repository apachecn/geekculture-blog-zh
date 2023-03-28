# LeetCode —在旋转排序数组 II 中搜索

> 原文：<https://medium.com/geekculture/leetcode-search-in-rotated-sorted-array-ii-9ffab3dbe926?source=collection_archive---------13----------------------->

# 问题陈述

有一个整数数组 *nums* 按非降序排序(不一定有**不同的**值)。

在传递给你的函数之前， *nums* 在一个未知的枢轴索引*k(0<= k<nums . length)*处**旋转**，这样得到的数组是*【nums[k]，nums[k + 1]，…，nums[n-1]，nums[0]，…，nums[1]，…，nums[k-1]]***(0-索引)**。例如，*【0，1，2，4，4，4，5，6，6，7】*可能在枢轴索引 5 处旋转，变成*【4，5，6，6，7，0，1，2，4，4】*。

给定旋转后的数组*nums*和一个整数 *target* ，如果 target 在 nums 中则返回 true，如果不在 nums 中则返回 false。

你必须尽可能减少整个操作步骤。

问题陈述摘自:[https://leet code . com/problems/search-in-rotated-sorted-array-ii/](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)。

**例 1:**

```
Input: nums = [2, 5, 6, 0, 0, 1, 2], target = 0
Output: true
```

**例 2:**

```
Input: nums = [2, 5, 6, 0, 0, 1, 2], target = 3
Output: false
```

**约束:**

```
- 1 <= nums.length <= 10^5
- -2^31 <= nums[i] <= 2^31 - 1
- 0 <= k <= 10^5
```

# 说明

该问题的解决方案类似于之前在旋转排序数组中的[搜索。唯一的区别是，由于存在重复项， **nums[low] == nums[mid]** 是一种可能性，我们必须单独处理这种情况。](https://alkeshghorpade.me/post/leetcode-search-in-rotated-sorted-array)

让我们直接跳到算法。

```
// search function
- if low > high
    - return -1- set mid = low + (high - low) / 2- if nums[mid] == key
    - return true- if nums[low] == nums[mid + 1] && nums[high] == nums[mid]
    - low++
    - high--
    - search(nums, low, high, key)if nums[low] <= nums[mid]
    - if key >= nums[low] && key <= nums[mid]
        - return search(nums, low, mid - 1, key) - return search(nums, mid + 1, high, key)if key >= nums[mid] && key <= nums[high]
    - return search(nums, mid + 1, high, key)- return search(nums, low, mid - 1, key)
```

让我们来看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    bool searchUtil(vector<int>& nums, int low, int high, int target) {
        if(low > high) {
            return false;
        } int mid = low + (high - low)/2; if(nums[mid] == target){
            return true;
        } if(nums[low] == nums[mid] && nums[high] == nums[mid]){
            low++;
            high--;
            return searchUtil(nums, low, high, target);
        } if(nums[low] <= nums[mid]){
            if(target >= nums[low] && target <= nums[mid]){
                return searchUtil(nums, low, mid - 1, target);
            } return searchUtil(nums, mid + 1, high, target);
        } if(target >= nums[mid] && target <= nums[high]){
            return searchUtil(nums, mid + 1, high, target);
        } return searchUtil(nums, low, mid - 1, target);
    } bool search(vector<int>& nums, int target) {
        bool result = searchUtil(nums, 0, nums.size() - 1, target);
        return result;
    }
};
```

## 戈朗溶液

```
func searchUtil(nums []int, low, high, target int) bool {
    if low > high {
        return false
    } mid := low + (high - low) / 2 if nums[mid] == target {
        return true
    } if nums[low] == nums[mid] && nums[high] == nums[mid] {
        low++
        high--
        return searchUtil(nums, low, high, target)
    } if nums[low] <= nums[mid] {
        if target >= nums[low] && target <= nums[mid] {
            return searchUtil(nums, low, mid - 1, target)
        } return searchUtil(nums, mid + 1, high, target)
    } if target >= nums[mid] && target <= nums[high] {
        return searchUtil(nums, mid + 1, high, target);
    } return searchUtil(nums, low, mid - 1, target);
}func search(nums []int, target int) bool {
    return searchUtil(nums, 0, len(nums) - 1, target)
}
```

## Javascript 解决方案

```
var searchUtil = function(nums, low, high, target) {
    if(low > high) {
        return false;
    } let mid = low + (high - low)/2; if(nums[mid] == target){
        return true;
    } if(nums[low] == nums[mid] && nums[high] == nums[mid]){
        low++;
        high--;
        return searchUtil(nums, low, high, target);
    } if(nums[low] <= nums[mid]){
        if(target >= nums[low] && target <= nums[mid]){
            return searchUtil(nums, low, mid - 1, target);
        } return searchUtil(nums, mid + 1, high, target);
    } if(target >= nums[mid] && target <= nums[high]){
        return searchUtil(nums, mid + 1, high, target);
    } return searchUtil(nums, low, mid - 1, target);
};var search = function(nums, target) {
    return searchUtil(nums, 0, nums.length - 1, target);
};
```

让我们试着解决这个问题。

```
Input: nums = [2, 5, 6, 0, 0, 1, 2], target = 3// search function
Step 1: searchUtil(nums, 0, nums.size() - 1, target)
        searchUtil(nums, 0, 6, 0)// searchUtil function
Step 2: low > high
        0 > 6
        falseStep 3: mid = low + (high - low)/2
            = 0 + (6 - 0)/2
            = 0 + 6/2
            = 3Step 4: if nums[mid] == target
           nums[3] == 3
           0 == 3
           falseStep 5: if nums[low] == nums[mid] && nums[high] == nums[mid]
           nums[0] == nums[3] && nums[6] == nums[3]
           2 == 0 && 2 == 0
           falseStep 6: if nums[low] <= nums[mid]
           nums[0] <= nums[3]
           2 <= 0
           falseStep 7: if target >= nums[mid] && target <= nums[high]
           3 >= nums[3] && 3 <= nums[6]
           3 >= 0 && 3 <= 2
           falseStep 8: searchUtil(nums, low, mid - 1, target)
        searchUtil(nums, 0, 2, 3)// searchUtil function
Step 9: low > high
        0 > 2
        falseStep 10: mid = low + (high - low)/2
             = 0 + (2 - 0)/2
             = 0 + 2/2
             = 1Step 11: if nums[mid] == target
            nums[1] == 3
            5 == 3
            falseStep 12: if nums[low] == nums[mid] && nums[high] == nums[mid]
            nums[0] == nums[1] && nums[2] == nums[1]
            2 == 5 && 6 == 5
            falseStep 13: if nums[low] <= nums[mid]
            nums[0] <= nums[1]
            2 <= 5
            true if target >= nums[low] && target <= nums[mid]
               3 >= nums[0] && 3 <= nums[1]
               3 >= 2 && 3 <= 5
               true return searchUtil(nums, low, mid - 1, target)
                      searchUtil(nums, 0, 1 - 1, 3)
                      searchUtil(nums, 0, 0, 3)// searchUtil function
Step 14: if low > high
            0 > 0
            falseStep 15: mid = low + (high - low)/2
             = 0 + (0 - 0)/2
             = 0 + 0/2
             = 0Step 16: if nums[mid] == target
            nums[0] == 3
            2 == 3
            falseStep 17: if nums[low] == nums[mid] && nums[high] == nums[mid]
            nums[0] == nums[0] && nums[0] == nums[0]
            2 == 2 && 2 == 2
            true low++
            low = 1 high--
            high = -1 return searchUtil(nums, low, high, target)
                   searchUtil(nums, 1, -1, 3)// searchUtil function
Step 14: if low > high
            1 > -1
            true return false// We go back from Step 14 to Step 9 to Step 2 to Step 1 because of recursionWe return the answer as false.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-search-in-sorted-rotated-array-ii)*。*