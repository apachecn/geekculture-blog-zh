# LeetCode —三个数的最大乘积

> 原文：<https://medium.com/geekculture/leetcode-maximum-product-of-three-numbers-a99912d9e22?source=collection_archive---------10----------------------->

# 问题陈述

给定一个整数数组 **nums** ，*找出乘积最大的三个数，并返回最大乘积*。

问题陈述摘自:[https://leet code . com/problems/maximum-product-of-three-numbers](https://leetcode.com/problems/maximum-product-of-three-numbers)

**例 1:**

```
Input: nums = [1, 2, 3]
Output: 6
```

**例 2:**

```
Input: nums = [1, 2, 3, 4] 
Output: 24
```

**例 3:**

```
Input: nums = [-1, -2, -3]
Output: -6
```

**约束:**

```
- 3 <= nums.length <= 10^4 
- -1000 <= nums[i] <= 1000
```

# 说明

我们有多种方法可以解决这个问题。让我们探索从最坏情况到最好情况的所有解决方案。

## 强力:3 个嵌套循环

一个简单的解决方案是使用三个嵌套循环检查数组的每个三元组。

这种方法的 C++代码片段如下所示:

```
for (int i = 0; i < n - 2; i++)
    for (int j = i + 1; j < n - 1; j++)
        for (int k = j + 1; k < n; k++)
            max_product = max(max_product, arr[i] * arr[j] * arr[k]);
```

如上所示，时间复杂度为 **O(N )** ，空间复杂度为 **O(1)** 。

## 使用额外空间

我们可以使用额外的空间将时间复杂度降低到 O(N)。

1.  我们构造了四个与输入数组大小相同的数组 leftMax[]、rightMax[]、leftMin[]和 rightMin[]。
2.  我们将上述四个数组填充如下:

*   leftMax[i]将包含 arr[i]左侧的最大元素，不包括 arr[i]。对于索引 0，left 将包含-1。
*   leftMin[i]将包含 arr[i]左侧的最小元素，不包括 arr[i]。对于索引 0，left 将包含-1。
*   rightMax[i]将包含 arr[i]右侧的最大元素，不包括 arr[i]。对于索引 n -1，right 将包含-1。
*   rightMin[i]将包含 arr[i]右侧的最小元素，不包括 arr[i]。对于索引 n -1，right 将包含-1。

3.对于除第一个和最后一个索引之外的所有数组索引 I，计算 arr[i] *x* y 的最大值，其中 x 可以是 leftMax[i]或 leftMin[i]，y 可以是 rightMax[i]或 rightMin[i]。

4.返回步骤 3 中的最大值。

这种方法的 C++代码片段如下所示:

```
vector<int> leftMin(n, -1);
vector<int> rightMin(n, -1);
vector<int> leftMax(n, -1);
vector<int> rightMax(n, -1);

for (int i = 1; i < n; i++){
    leftMax[i] = max_sum;
    if (arr[i] > max_sum)
        max_sum = arr[i];

    leftMin[i] = min_sum;
    if (arr[i] < min_sum)
        min_sum = arr[i];
}

for (int j = n - 2; j >= 0; j--){
    rightMax[j] = max_sum;
    if (arr[j] > max_sum)
        max_sum = arr[j];

    rightMin[j] = min_sum;
    if (arr[j] < min_sum)
        min_sum = arr[j];
}

for (int i = 1; i < n - 1; i++){
    int max1 = max(arr[i] * leftMax[i] * rightMax[i], arr[i] * leftMin[i] * rightMin[i]);
    int max2 = max(arr[i] * leftMax[i] * rightMin[i], arr[i] * leftMin[i] * rightMax[i]);

    max_product = max(max_product, max(max1, max2));
}
```

该方法的空间复杂度为 O(N) 。

## 使用排序

我们可以通过对数组排序来降低空间复杂度，并考虑数组最后三个元素的乘积与前两个元素和最后一个元素的乘积之间的最大值。

这种方法的 C++代码片段如下所示:

```
sort(arr, arr + n);

return max(arr[0] * arr[1] * arr[n - 1], arr[n - 1] * arr[n - 2] * arr[n - 3]);
```

该方法的时间复杂度为 **O(NlogN)** ，空间复杂度为 **O(N)** 。

## 使用五个变量

这个问题可以用五个变量来解决。三个变量将在一个数组中存储最大值。其余两个将存储数组中的最小值。

让我们检查算法:

```
- set max1, max2 and max3 to INT_MIN
  set min1, min2 to INT_MAX

- loop for i = 0; i < nums.size(); i++
  - if nums[i] < min1
    - set min2 = min1
    - set min1 = nums[i]
  - else if nums[i] < min2
    - set min2 = nums[i]

  - if nums[i] > max1
    - set max3 = max2
    - set max2 = max1
    - set max1 = nums[i]
  - else if nums[i] > max2
    - set max3 = max2
    - set max2 = nums[i]
  - else if nums[i] > max3
    - set max3 = nums[i]

- return max(min1 * min2 * max1, max1 * max2 * max3)
```

## C++解决方案

```
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN;
        int min1 = INT_MAX, min2 = INT_MAX;

        for(int i = 0; i < nums.size(); i++){
            if(nums[i] < min1){
                min2 = min1;
                min1 = nums[i];
            } else if(nums[i] < min2){
                min2 = nums[i];
            }

            if(nums[i] > max1){
                max3 = max2;
                max2 = max1;
                max1 = nums[i];
            } else if(nums[i] > max2){
                max3 = max2;
                max2 = nums[i];
            } else if(nums[i] > max3){
                max3 = nums[i];
            }
        }

        return max(min1*min2*max1, max1*max2*max3);
    }
};
```

## 戈朗溶液

```
const MAXINT = math.MaxInt32
const MININT = math.MinInt32

func maximumProduct(nums []int) int {
    max1, max2, max3 := MININT, MININT, MININT
    min1, min2 := MAXINT, MAXINT

    for i := 0; i < len(nums); i++ {
        if nums[i] < min1 {
            min2 = min1
            min1 = nums[i]
        } else if nums[i] < min2 {
            min2 = nums[i]
        }

        if nums[i] > max1 {
            max3 = max2
            max2 = max1
            max1 = nums[i]
        } else if nums[i] > max2 {
            max3 = max2
            max2 = nums[i]
        } else if nums[i] > max3 {
            max3 = nums[i]
        }
    }

    return int(math.Max(float64(min1 *min2 * max1), float64(max1 * max2 * max3)))
}
```

## Javascript 解决方案

```
var maximumProduct = function(nums) {
    let min1 = Number.POSITIVE_INFINITY, min2 = Number.POSITIVE_INFINITY;
    let max1 = Number.NEGATIVE_INFINITY, max2 = Number.NEGATIVE_INFINITY, max3 = Number.NEGATIVE_INFINITY;

    for(let i = 0; i < nums.length; i++) {
        if( nums[i] < min1 ) {
            min2 = min1;
            min1 = nums[i];
        } else if( nums[i] < min2 ) {
            min2 = nums[i];
        }

        if( nums[i] > max1 ) {
            max3 = max2;
            max2 = max1;
            max1 = nums[i];
        } else if( nums[i] > max2 ) {
            max3 = max2;
            max2 = nums[i];
        } else if( nums[i] > max3 ) {
            max3 = nums[i];
        }
    }

    return Math.max(min1 * min2 * max1, max1 * max2 * max3 );
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [-6, 5, 1, 2, 3, -4, 9]

Step 1: int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN;
        int min1 = INT_MAX, min2 = INT_MAX;

Step 2: loop for int i = 0; i < nums.size()
        i < nums.size()
        0 < 7
        true

        if nums[i] < min1
           nums[0] < INT_MAX
           -6 < INT_MAX
           true

           - min2 = min1
                  = INT_MAX
           - min1 = nums[i]
                  = nums[0]
                  = -6

        if nums[i] > max1
           nums[0] > INT_MIN
           -6 > INT_MIN
           true

           - max3 = max2
                  = INT_MIN
           - max2 = max1
                  = INT_MIN
           - max1 = nums[i]
                  = nums[0]
                  = -6

        i++
        i = 1

Step 3: loop for int i = 0; i < nums.size()
        i < nums.size()
        1 < 7
        true

        if nums[i] < min1
           nums[1] < INT_MAX
           1 < -6
           false
        else if nums[i] < min2
           5 < INT_MAX
           true

           - min2 = nums[i]
                  = 5

        if nums[i] > max1
           nums[1] > -6
           5 > -6
           true

           - max3 = max2
                  = INT_MIN
           - max2 = max1
                  = -6
           - max1 = nums[i]
                  = nums[1]
                  = 5

        i++
        i = 2

Step 4: loop for int i = 0; i < nums.size()
        i < nums.size()
        2 < 7
        true

        if nums[i] < min1
           nums[2] < -6
           1 < -6
           false
        else if nums[i] < min2
           1 < 5
           true

           - min2 = nums[2]
                  = 1

        if nums[i] > max1
           nums[2] > 5
           1 > 5
           false

        else if nums[i] > max2
           nums[2] > -6
           1 > -6
           true

           - max3 = max2
                  = -6
           - max2 = nums[i]
                  = nums[2]
                  = 1

        i++
        i = 3

Step 5: loop for int i = 0; i < nums.size()
        i < nums.size()
        3 < 7
        true

        if nums[i] < min1
           nums[3] < -6
           2 < -6
           false
        else if nums[i] < min2
           2 < 1
           false

        if nums[i] > max1
           nums[3] > 5
           2 > 5
           false

        else if nums[i] > max2
           nums[3] > 1
           2 > 1
           true

           - max3 = max2
                  = 1
           - max2 = nums[i]
                  = nums[3]
                  = 2

        i++
        i = 4

Step 6: loop for int i = 0; i < nums.size()
        i < nums.size()
        4 < 7
        true

        if nums[i] < min1
           nums[4] < -6
           3 < -6
           false
        else if nums[i] < min2
           3 < 1
           false

        if nums[i] > max1
           nums[4] > 5
           3 > 5
           false

        else if nums[i] > max2
           nums[4] > 2
           3 > 2
           true

           - max3 = max2
                  = 2
           - max2 = nums[i]
                  = nums[4]
                  = 3

        i++
        i = 5

Step 7: loop for int i = 0; i < nums.size()
        i < nums.size()
        5 < 7
        true

        if nums[i] < min1
           nums[5] < -6
           -4 < -6
           false
        else if nums[i] < min2
           -4 < 1
           true

           - min2 = nums[i]
                  = -4

        if nums[i] > max1
           nums[5] > 5
           -4 > 5
           false

        else if nums[i] > max2
           nums[5] > 3
           -4 > 3
           false

        else if nums[i] > max3
           nums[5] > 2
           -4 > 2
           false

        i++
        i = 6

Step 8: loop for int i = 0; i < nums.size()
        i < nums.size()
        6 < 7
        true

        if nums[i] < min1
           nums[6] < -6
           9 < -6
           false
        else if nums[i] < min2
           9 < -4
           false

        if nums[i] > max1
           nums[6] > 5
           9 > 5
           true

           - max3 = max2
                  = 3
           - max2 = max1
                  = 5
           - max1 = nums[i]
                  = nums[6]
                = 9

        i++
        i = 7

Step 9: loop for int i = 0; i < nums.size()
        i < nums.size()
        7 < 7
        false

Step 10: return max(min1 * min2 * max1, max1 * max2 * max3)
                max(-6 * -4 * 9, 9 * 5 * 3)
                max(216, 135)
                = 216

So we return the answer as 216.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-maximum-product-of-three-numbers)*。*