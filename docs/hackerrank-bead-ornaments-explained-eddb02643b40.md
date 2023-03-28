# HackerRank 珠饰品说明

> 原文：<https://medium.com/geekculture/hackerrank-bead-ornaments-explained-eddb02643b40?source=collection_archive---------19----------------------->

## 理解凯莱公式和**生成树的基尔霍夫定理**

![](img/da7f6bb4974ffc1f732c3ab88f62731b.png)

Photo by [Cookie the Pom](https://unsplash.com/@cookiethepom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是一个有趣的问题。但是看起来像是背 Linux 命令。如果你没有图形算法的背景，这很难解决。如果你知道这些算法，包括[凯莱公式](https://en.wikipedia.org/wiki/Cayley%27s_formula)和[基尔霍夫定理](https://en.wikipedia.org/wiki/Kirchhoff%27s_theorem)，你可以用这个公式问题分分钟求解。

## 问题

珠子有 N 种颜色。你有第 I 种颜色的珠子。你想把所有的珠子连在一起做成一件装饰品。您可以使用以下算法创建装饰:

*   步骤# 1:将所有的珠子按任意顺序排列，这样相同颜色的珠子就可以放在一起。
*   第二步:饰品最初只由排列中的第一颗珠子组成。
*   第三步:对于顺序中的每一个珠子，把它和装饰物中相同颜色的珠子连接起来。如果没有相同颜色的珠子，它可以连接到饰品中的任何珠子上。

所有的珠子都是截然不同的，即使它们有相同的颜色。如果两个珠子在一种结构中用线连接，而在另一种结构中没有，那么这两个饰品被认为是不同的。使用这种算法可以形成多少种不同的饰品？返回答案模(1000000007L)。

**更新/澄清**

把珠子的形成想象成一棵树，而不是一条直线。一个珠子可以连接任意数量的珠子。

[](https://www.hackerrank.com/challenges/beadornaments/problem) [## 珠子饰品| HackerRank

### 珠子有颜色。你有这种颜色的珠子。你想把所有的珠子连在一起做成一件装饰品…

www.hackerrank.com](https://www.hackerrank.com/challenges/beadornaments/problem) 

## 解决办法

请不要用排列混淆它。这是一个图算法问题。如果你没有这方面的背景，你可能不需要去理解它，因为这些图论定理可能需要数学家花费数年的时间来建立。坦率地说，这可能不是一个好的面试问题，但它只是测试你是否知道这些图表公式。

[Stackoverflow](https://stackoverflow.com/questions/23327244/bead-ornaments-algorithm) 对这个问题有很好的解释。这里就不赘述了。但这里将举例说明用 Java 实现，以便于理解。

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

    /*
     * Complete the 'beadOrnaments' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER_ARRAY b as parameter.
     */

    final static long MOD = 1000000007L;
    public static int beadOrnaments(List<Integer> b) {
        // Write your code here
        if (b.size() < 1) return 0;

        long res = 0;
        if (b.size() == 1) {
            // n = 1: This is a problem of Cayley's formula
            int a = b.get(0);
            res = (long) Math.pow(a, a - 2);   
        }
        else {
            // n > 1: it's Kirchhoff's matrix tree with Cayley's formula
            long sum = 0;
            res = 1;
            for (int v : b) {
                res *= Math.pow(v, v - 1);
                sum += v;
            }
            res *= Math.pow(sum, b.size() - 2);
        }

        return (int) (res % MOD);
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, t).forEach(tItr -> {
            try {
                int bCount = Integer.parseInt(bufferedReader.readLine().trim());

                List<Integer> b = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                    .map(Integer::parseInt)
                    .collect(toList());

                int result = Result.beadOrnaments(b);

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

看起来很简洁。编码快乐！

*问题、想法？在这里留下评论。跟随我成为有趣的解决问题之旅的一部分。*