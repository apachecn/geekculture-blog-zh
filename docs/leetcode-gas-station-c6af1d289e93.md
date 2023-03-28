# LeetCode —加油站

> 原文：<https://medium.com/geekculture/leetcode-gas-station-c6af1d289e93?source=collection_archive---------18----------------------->

# 问题陈述

环形路线上有 *n* 个加油站，其中*第*站的气量为*气【I】*。

你有一辆汽车，油箱是无限的，从第*个*站到下一个 *(i + 1)个*站需要花费* cost[I]*的汽油。你在一个加油站带着一个空油箱开始旅程。

给定两个整数数组 *gas* 和 *cost* ，如果可以顺时针方向绕圈一圈，则返回*起始加油站的索引，否则返回-1* 。如果存在解决方案，则**保证**是**唯一的**。

问题陈述摘自:[https://leetcode.com/problems/gas-station](https://leetcode.com/problems/gas-station)

**例 1:**

```
Input: gas = [1, 2, 3, 4, 5], cost = [3, 4, 5, 1, 2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4\. Your tank = 4 - 1 + 5 = 8
Travel to station 0\. Your tank = 8 - 2 + 1 = 7
Travel to station 1\. Your tank = 7 - 3 + 2 = 6
Travel to station 2\. Your tank = 6 - 4 + 3 = 5
Travel to station 3\. The cost is 5\. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

**例 2:**

```
Input: gas = [2, 3, 4], cost = [3, 4, 3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0\. Your tank = 4 - 3 + 2 = 3
Travel to station 1\. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 units of gas, but you only have 3.
Therefore, you can't travel around the circuit once, no matter where you start.
```

**约束:**

```
- gas.length == n
- cost.length == n
- 1 <= n <= 10^5
- 0 <= gas[i], cost[i] <= 10^4
```

# 说明

## 强力方法

蛮力的解决方法是把每一个加油站都当成一个起点，看看是否有可能的完整回路。

为了检查每个站点，我们将使用两个嵌套循环。

```
int start = 0;
int end = 1;

int current_petrol = gas[start] - cost[start];

while (end != start || current_petrol < 0) {
    while (current_petrol < 0 && start != end) {
        current_petrol -= gas[start] - cost[start];
        start = (start + 1) % n;

        if (start == 0)
            return -1;
    }

    current_petrol += gas[start] - cost[start];
    end = (end + 1) % n;
}

return start;
```

上述方法的时间复杂度为 **O(N )** 。

## 优化解决方案

如果我们看问题，我们关注的点应该是天然气的总量应该大于总成本。我们需要考虑的唯一因素是设置起点。

我们直接跳到算法。

```
- set current, total, start and diff to 0

- loop for i = 0; i < gas.size(); i++
  - diff = gas[i] -  cost[i]

  - total += diff
  - current += diff

  // current value refers to the total gas - total cost till ith station.
  // if that value is negative, this means gas is exhausted, and we cannot complete the circuit.
  // Hence we set the current index as the start.
  - if current < 0
    - start = i + 1
    - current = 0

- if total >= 0
  - return start

- return -1
```

## C++解决方案

```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int current = 0;
        int total = 0;
        int diff = 0;
        int start = 0;

        for(int i = 0; i < gas.size(); i++){
            diff = gas[i] - cost[i];

            total += diff;
            current += diff;
            if(current < 0){
                start = i + 1;
                current = 0;
            }
        }

        if(total >= 0){
            return start;
        }

        return -1;
    }
};
```

## 戈朗溶液

```
func canCompleteCircuit(gas []int, cost []int) int {
    current, total, start, diff := 0, 0, 0, 0

    for i := 0; i < len(gas); i++ {
        diff = gas[i] - cost[i]

        total += diff
        current += diff

        if current < 0 {
            start = i + 1
            current = 0
        }
    }

    if total >= 0 {
        return start
    }

    return -1
}
```

## Javascript 解决方案

```
var canCompleteCircuit = function(gas, cost) {
    let start = 0, diff = 0, total = 0, current = 0;

    for( let i = 0; i < gas.length; i++ ) {
        diff = gas[i] - cost[i];

        total += diff;
        current += diff;

        if( current < 0 ) {
            start = i + 1;
            current = 0;
        }
    }

    if( total >= 0 ) {
        return start;
    }

    return -1;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: gas = [1, 2, 3, 4, 5], cost = [3, 4, 5, 1, 2]

Step 1: current = 0
        total = 0
        diff = 0
        start = 0

Step 2: loop for i = 0; i < gas.size()
        i < gas.size()
        0 < 5
        true

        diff = gas[i] - cost[i]
             = gas[0] - cost[0]
             = 1 - 3
             = -2

        total = total + diff
              = 0 - 2
              = -2

        current = current + diff
                = 0 - 2
                = -2

        if current < 0
          - -2 < 0
          - true

            start = i + 1
                  = 0 + 1
                  = 1

            current = 0

        i++
        i = 1

Step 3: i < gas.size()
        1 < 5
        true

        diff = gas[i] - cost[i]
             = gas[1] - cost[1]
             = 2 - 4
             = -2

        total = total + diff
              = -2 + (-2)
              = -4

        current = current + diff
                = 0 - 2
                = -2

        if current < 0
          - -2 < 0
          - true

            start = i + 1
                  = 1 + 1
                  = 2

            current = 0

        i++
        i = 2

Step 4: i < gas.size()
        2 < 5
        true

        diff = gas[i] - cost[i]
             = gas[2] - cost[2]
             = 3 - 5
             = -2

        total = total + diff
              = -4 + (-2)
              = -6

        current = current + diff
                = 0 - 2
                = -2

        if current < 0
          - -2 < 0
          - true

            start = i + 1
                  = 2 + 1
                  = 3

            current = 0

        i++
        i = 3

Step 5: i < gas.size()
        3 < 5
        true

        diff = gas[i] - cost[i]
             = gas[3] - cost[3]
             = 4 - 1
             = 3

        total = total + diff
              = -6 + 3
              = -3

        current = current + diff
                = 0 + 3
                = 3

        if current < 0
          - 3 < 0
          - false

        i++
        i = 4

Step 6: i < gas.size()
        4 < 5
        true

        diff = gas[i] - cost[i]
             = gas[4] - cost[4]
             = 5 - 2
             = 3

        total = total + diff
              = -3 + 3
              = 0

        current = current + diff
                = 3 + 3
                = 6

        if current < 0
          - 6 < 0
          - false

        i++
        i = 5

Step 7: i < gas.size()
        5 < 5
        false

Step 8: if total >= 0
          - 0 >= 0
          - true

Step 9: return start

So the answer we return is 3.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-gas-station)*。*