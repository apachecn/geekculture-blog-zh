# 了解 HackerRank 乐高积木

> 原文：<https://medium.com/geekculture/understand-hackerrank-lego-blocks-bca16e1dc065?source=collection_archive---------1----------------------->

## 洞察 HackerRank 乐高积木问题

![](img/7c153adab24da110af02e5b1270e8d56.png)

Photo by [Billy Huynh](https://unsplash.com/@billy_huy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **问题**

你有无限数量的 4 种乐高积木，尺寸如下(深 x 高 x 宽):

```
d	h	w
1	1	1
1	1	2
1	1	3
1	1	4
```

用这些砖块，你要做一面高 *n* 宽 *m* 的墙。该墙的特点是:

-墙上不应该有任何洞。
-你建造的墙应该是一个坚固的结构，所以不应该有垂直的裂缝穿过所有的砖块。砖块必须水平放置。

有多少种方法可以建造这堵墙？

**例子**

```
n = 2
m = 3
```

高度为 2，宽度为 3。总共有 9 种有效排列。

**功能描述**

在下面的编辑器中完成乐高积木功能。

legoBlocks 具有以下参数:

*   int n:墙的高度
*   int m:墙的宽度

**返回**
- int:有效墙形成数模(10⁹+7)

**输入格式**

第一行包含测试用例 *t* 的数量。

接下来的每一行都包含两个空格分隔的整数 *n* 和 *m* 。

**约束条件**

```
1 <= t <= 100
1 <= n,m <= 1000
```

见下面 HackerRack 原问题:

[](https://www.hackerrank.com/challenges/lego-blocks/problem) [## 乐高积木| HackerRank

### 你有无限数量的 4 种乐高积木，尺寸如下(深×高×宽):使用这些积木…

www.hackerrank.com](https://www.hackerrank.com/challenges/lego-blocks/problem) 

## 解决办法

有一个 Python 解决方案如下:

```
def legoBlocks(n, m):
    MOD = (10**9 +7)

    # Step 1: O(W)       
    # The number of combinations to build a single row
    # As only four kinds of sizes, so
    # base case: 
    # if width is 0, combination is 1
    # if width is 1, combination is 1
    # if width is 2, combination is 2
    # if width is 3, combination is 4
    row_combinations = [1, 1, 2, 4]

    # Build row combinations up to current wall's width
    while len(row_combinations) <= m: 
        row_combinations.append(sum(row_combinations[-4:]) % MOD)

    # Step 2: O(W)
    # Compute total combinations 
    # for constructing a wall of height N of varying widths
    total = [pow(c, n, MOD) for c in row_combinations]

    # Step 3: O(W^2)
    # Find the number of unstable wall configurations 
    # for a wall of height N of varying widths
    unstable = [0, 0]

    # Divide the wall into left part and right part,
    # and calculate the combination of left part and right part.
    # From width is 2, we start to consider about violation.
    for i in range(2, m + 1):
        # i: current total width
        # j: left width
        # i - j: right width
        # f: (left part - previous vertical violation)*right part
        f = lambda j: (total[j] - unstable[j]) * total[i - j]
        result = sum(map(f, range(1, i)))
        unstable.append(result % MOD)

    # Print the number of stable wall combinations
    return (total[m] - unstable[m]) % MOD
```

解决这个问题需要 3 个步骤:

1.  只考虑一行的所有情况
2.  扩展到所有行
3.  减去垂直不稳定的情况

有两种方法可以理解上面的步骤 3。不稳定可以通过对每个(稳定*总计)(**解 1** )求和来计算，或者通过对每个(结果*总计)(**解 2** )求和来立即计算结果。下面详细请看 Java 来举例说明。请注意两个解决方案的注释中的“解决方案 1”和“解决方案 2”。

```
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class Result {

// - Step 1: consider only one row
// - Step 2: extend to all rows
// - Step 3: subtract the vertically unstable
// The unstable is calculated by summing each stable*total
// or calculate the result immediately by summing each result*total

    /*
     * Complete the 'legoBlocks' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts the following parameters:
     *  1\. INTEGER n - height
     *  2\. INTEGER m - width
     */

    public static int legoBlocks(int n, int m) {
        // Write your code here
        if (n < 2 || m < 1) return 0;
        if (m == 1) return 1;

        // - Step 1: consider only one row
        long [] total = new long [m + 1];

        // set a flag (-1) to calculate only once
        for (int i = 0; i < total.length; i++)
            total[i] = -1;

        fillTot(total, m);

        // - Step 2: extend to all rows
        for (int i = 0; i < total.length; i++) {
            long tmp = 1;
            for (int j = 0; j < n; j++) {
                tmp = (tmp * total[i]) % MOD;
            }
            total[i] = tmp;
        }

        // - Step 3: subtract the vertically unstable
        // don't calculate the vertically unstable at first
        long [] result = new long [m + 1];
        // set a flag (-1) to calculate only once
        for (int i = 0; i < result.length; i++)
            result[i] = -1;

        getResult(total, result, m);

        // solution 1:
        // - subtract the vertically unstable
        // return (int) ((total[m] - result[m]) % MOD);

        // solution 2:
        // - return the result
        return (int) (result[m] % MOD);
    }

    static long MOD = 1000000000 + 7;

    // calculate unstable by splitting it into two parts and
    // multiplying unstable part with total part
    static long getResult(long [] total, long [] result, int i) {
        if (result[i] == -1) {
            if (i == 1) {
                // solution 1
                // result[i] = 0;

                // solution 2
                result[i] = 1;
            }
            else {
                // solution 1
                // result[i] = 0;
                // for (int j = 1; j < i; j++) {
                //     result[i] += ((total[j] - getResult(total, result, j)) * total[i - j]) % MOD;
                // }

                // solution 2            
                result[i] = total[i];
                for (int j = 1; j < i; j++) {
                    result[i] -= (getResult(total, result, j) * total[i - j]) % MOD;
                }
            }
        }

        return result[i];
    }

    // fill totals partially
    static long fillTot(long [] total, int i) {
        if (i < 0) return 0;

        if (total[i] == -1) {
            if (i == 0 || i == 1) 
                total[i] = 1;
            else 
                total[i] = (fillTot(total, i - 1) + fillTot(total, i - 2) + fillTot(total, i - 3) + fillTot(total, i - 4)) % MOD;
        }

        return total[i];
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, t).forEach(tItr -> {
            try {
                String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

                int n = Integer.parseInt(firstMultipleInput[0]);

                int m = Integer.parseInt(firstMultipleInput[1]);

                int result = Result.legoBlocks(n, m);

                bufferedWriter.write(String.valueOf(result));
                bufferedWriter.newLine();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

编码快乐！