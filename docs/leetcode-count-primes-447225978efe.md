# LeetCode —计算素数

> 原文：<https://medium.com/geekculture/leetcode-count-primes-447225978efe?source=collection_archive---------6----------------------->

# 问题陈述

给定一个整数 *n* ，返回*严格小于 n* 的素数个数。

**例一:**

```
Input: n = 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

**例 2:**

```
Input: n = 0
Output: 0
```

**例 3:**

```
Input: n = 1
Output: 0
```

**约束条件**

```
- 0 <= n <= 5 * 10^6
```

# 说明

## 强力方法

最简单的解决方案是检查从 3 到 n 的每个数字，并验证它是否是质数。

这种方法的 C++代码片段如下所示:

```
bool isPrime(int n){
    if (n <= 1)
        return false; for (int i = 2; i < n; i++)
        if (n % i == 0)
            return false; return true;
}int printPrime(int n){
    // since 2 is prime we set count to 1
    int count = 1; for (int i = 3; i < n; i++) {
        if (isPrime(i))
            count++;
    } return count;
}
```

上述方法的时间复杂度为 **O(N )** 。

## 平方根法

通过迭代直到数的平方根，可以进一步优化这种粗暴的方法。

让我们用 C++来检验这种方法。

```
bool isPrime(int n){
    if (n <= 1)
        return false;
    if (n <= 3)
        return true; if (n % 2 == 0 || n % 3 == 0)
        return false; for (int i = 5; i * i <= n; i = i + 6)
        if (n % i == 0 || n % (i + 2) == 0)
            return false; return true;
}int printPrime(int n){
    int count = 0; for (int i = 2; i < n; i++) {
        if (isPrime(i))
            count++;
    } return count;
}
```

这种方法的时间复杂度降低到**o(n^(3/2)**。

## 厄拉多塞筛

最好的方法是使用厄拉多塞算法的筛子。

让我们检查算法:

```
- return 0 if n <= 2- initialize bool primes array
  bool primes[n]- initialize i, j and set count = 0- set all primes array value to true
  - loop for i = 2; i < n; i++
    - primes[i] = true- loop for i = 2; i <= sqrt(n); i++
  - if primes[i]
    - loop for j = i + i; j < n; j += i
      - primes[j] = false- count all primes[i] = true
  loop for i = 2; i < n; i++
    - if primes[i]
      - count = count + 1- return count
```

该方法的时间复杂度为 **O(N * loglog(N))** 。

## C++解决方案

```
class Solution {
public:
    int countPrimes(int n) {
        if(n <= 2)
            return 0; bool primes[n];
        int i, j, count = 0; for(i = 2; i < n; i++){
            primes[i] = true;
        } for(i = 2; i <= sqrt(n); i++){
            if(primes[i]){
                for(j = i+i; j < n; j += i)
                    primes[j] = false;
            }
        } for(i = 2; i < n; i++)
            if(primes[i])
                count++; return count;
    }
};
```

## 戈朗溶液

```
func countPrimes(n int) int {
    if n <= 2 {
        return 0
    } primes := make([]bool, n)
    count := 0 for i := 2; i < n; i++ {
        primes[i] = true
    } for i := 2; i <= int(math.Sqrt(float64(n))); i++ {
        if primes[i] {
            for j := i+i; j < n; j += i {
                primes[j] = false
            }
        }
    } for i := 2; i < n; i++{
        if primes[i] {
            count++
        }
    } return count
}
```

## Javascript 解决方案

```
var countPrimes = function(n) {
    if( n <= 2 ) {
        return 0;
    } let primes = [];
    let i, j, count = 0; for( i = 2; i < n; i++ ){
        primes[i] = true;
    } for( i = 2; i <= Math.sqrt(n); i++ ){
        if( primes[i] ){
            for( j = i + i; j < n; j += i )
                primes[j] = false;
        }
    } for( i = 2; i < n; i++ )
        if( primes[i] )
            count++; return count;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: n = 10Step 1: if n <= 2
        10 < 2
        falseStep 2: bool primes[n]
        int i, j, count = 0Step 3: loop for(i = 2; i < n; i++){
            primes[i] = true
        } so all values from primes[0] to primes[9] are set to true.Step 4: loop for i = 2; i <= sqrt(n)
          i <= sqrt(10)
          2 <= 3
          true if primes[2]
            true loop for j = i + i; j < n
            j = i + i
              = 4 j < n
            4 < 10
            true primes[j] = false
            primes[4] = false j += i
            j = j + i
              = 4 + 2
              = 6 j < n
            6 < 10
            true primes[j] = false
            primes[6] = false j += i
            j = j + i
              = 6 + 2
              = 8 j < n
            8 < 10
            true primes[j] = false
            primes[8] = false j += i
            j = j + i
              = 8 + 2
              = 10 j < n
            10 < 10
            false i++
            i = 3Step 5: i <= sqrt(10)
        3 <= 3
        true if primes[3]
            true loop for j = i + i; j < n
             j = i + i
               = 6 j < n
        6 < 10
        true primes[j] = false
        primes[6] = false j += i
        j = j + i
          = 6 + 3
          = 9 j < n
        9 < 10
        true primes[j] = false
        primes[9] = false j += i
        j = j + i
          = 9 + 3
          = 12 j < n
        12 < 10
        false i++
        i = 4Step 6: i <= sqrt(10)
        4 <= 3
        falseStep 7: loop for i = 2; i < n; i++
              if primes[i]
                count++ primes from index 2
        = [true, true, false, true, false, true, false, false]
        so count of true is 4.Step 8: return countSo we return the answer as 4.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-count-primes)*。*