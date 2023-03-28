# FAANG 访谈摘要:Trie 难题

> 原文：<https://medium.com/geekculture/faang-interview-recap-trie-hard-problem-1db5f22216a2?source=collection_archive---------18----------------------->

## 征服 Trie 被 FAANG(或 MAMAA)问的难面试问题

![](img/6db98ae60119b7bdb771c77b88eda061.png)

Photo by [Jeremy Bishop](https://unsplash.com/@jeremybishop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Trie 是一种类型的 [*k* -ary](https://en.wikipedia.org/wiki/M-ary_tree) [搜索树](https://en.wikipedia.org/wiki/Search_tree)，一种用于从集合中定位特定关键字的树数据结构。它是一种重要的数据结构，用于建议性搜索和推荐。

这是一个常见但很难回答的面试问题。这些问题通常与不同非树数据结构中的字符或子串匹配或搜索有关，如矩阵和数组。对于优化的解决方案(即，能够被面试官接受)，我们需要将原始数据结构重组为一个 trie，然后我们可以快速解决问题。

这里让我们用三个真实的例子(面试问题)来看看如何有效地使用 trie 数据结构。

## 真实的例子

**问题 0** : HackerRank 无前缀集是典型的 Trie 面试问题。请在下面找到问题和优化的解决方案:

[](/geekculture/hackerrank-no-prefix-set-solution-6ddd99a7d00) [## HackerRank 无前缀集解决方案

### 解决 HackerRank 无前缀集问题

medium.com](/geekculture/hackerrank-no-prefix-set-solution-6ddd99a7d00) 

**问题 1** : HackerRank Contacts 问题依赖于 Trie 实现，否则很难优化。应该在其搜索部分使用 Trie DS(查找部分)。请看下面的问题。

[](https://www.hackerrank.com/challenges/contacts/problem) [## 联系人|黑客排名

### 我们将制作自己的联系人应用程序！应用程序必须执行两种类型的操作:添加名称，其中…

www.hackerrank.com](https://www.hackerrank.com/challenges/contacts/problem) 

请在下面找到优化的 Java 实现:

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
     * Complete the 'contacts' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts 2D_STRING_ARRAY queries as parameter.
     */

    public static List<Integer> contacts(List<List<String>> queries) {
    // Write your code here
        List<Integer> res = new ArrayList<>();

        // List<String> names = new ArrayList<>();
        Trie trie = new Trie('*');
        trie.cnt = 0;
        for (List<String> query : queries) {
            if (query.get(0).equals("add")) {
                // names.add(query.get(1));
                trie.add(query.get(1));           
            }   
            else if (query.get(0).equals("find")) {
                String q = query.get(1);  
                // int cnt = 0;
                // for (String name : names) {
                //     if (name.startsWith(q)) cnt++;
                // }

                res.add(trie.find(q));
            }
        }

        return res;    
    }

}

class Trie {
    char c;
    int cnt;
    Trie [] children;
    Trie(char a) {
        c = a;
        cnt = 1;
        children = new Trie[26];
    }

    Trie add(char a) {
        if (children[a - 'a'] == null) {
            children[a - 'a'] = new Trie(a);    
        }
        else {
            children[a - 'a'].cnt++;        
        }

        return children[a - 'a'];
    }

    void add(String s) {
        Trie node = this;
        for (int i = 0; i < s.length(); i++) {
            node = node.add(s.charAt(i));
        }
    }

    int find(String s) {
        Trie node = this;
        for (int i = 0; i < s.length(); i++) {
            node = node.children[s.charAt(i) - 'a'];
            if (node != null) {
                if (i == s.length() - 1) {
                    return node.cnt;
                }     
            }
            else 
                break;
        }
        return 0;   
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int queriesRows = Integer.parseInt(bufferedReader.readLine().trim());

        List<List<String>> queries = new ArrayList<>();

        IntStream.range(0, queriesRows).forEach(i -> {
            try {
                queries.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        List<Integer> result = Result.contacts(queries);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

核心是在一个树中搜索，而不是在一个数组中。在上面的代码中，您可以看到从数组 DS(注释掉)到 Trie DS 的变化。时间复杂度为 O(nlogn)。所以你可以用其他的数组，比如排序数组，然后用二分搜索法。

**问题 2** :这里以后会补充更多，或者请留下你对问题的回复。

## TL；DR；

所以我们可以看到这些难的面试问题的关键是如何构造一个 Trie 数据结构。然后，我们可以将复杂度从 O(n)降低到 O(nlogn)。对于这些问题，我们通常使用递归方法来降低代码复杂度，使其易于理解。但是如果可能的话请不要忘记缓存计算结果，这在算法上叫做 DP(动态规划)。

这篇文章将是继续添加更多与 Trie 相关的问题的一站。因此，请关注我，继续关注或查看更新。

快乐编码和归纳求解！

*问题、想法？在这里留下评论。跟随我成为有趣的解决问题之旅的一部分。*