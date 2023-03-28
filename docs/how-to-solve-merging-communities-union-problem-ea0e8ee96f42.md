# 如何解决合并社区联盟问题

> 原文：<https://medium.com/geekculture/how-to-solve-merging-communities-union-problem-ea0e8ee96f42?source=collection_archive---------10----------------------->

## 不使用库解决合并联合问题

![](img/94468471d4106758c06224575da7f879.png)

Photo by [Roman Synkevych 🇺🇦](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

合并是一个常见的问题。对此有许多解决方案。特别是一些高级编程语言或脚本都有现有的库可以使用。但是如何自己解决这个问题呢？

## 问题

人们在社交网络中相互联系。人 I 和人 j 之间的联系被表示为 Mij。当属于不同社区的两个人连接时，净效应是 I 和 j 所属的社区的合并。

一开始，有 n 个人代表 n 个社区。假设人 1 和 2 连接，然后 2 和 3 连接，那么 1、2 和 3 将属于同一个社区。

有两种类型的查询:

1.  Mij:如果包含人 I 和 j 的社区属于不同的社区，则它们被合并。
2.  齐:打印我所属社区的大小。

HackerRank 原问题如下:

[](https://www.hackerrank.com/challenges/merging-communities/problem) [## 合并社区| HackerRank

### 人们在社交网络中相互联系。人与人之间的联系被表示为。当两个…

www.hackerrank.com](https://www.hackerrank.com/challenges/merging-communities/problem) 

## 解决办法

如上所述，有多种实现方式。这里将举例说明两种解决方案。但是在 Java 中，合并树是最有效的方法之一。Java 中没有这方面的现有数据结构。

版本 1 :使用 Java HashSet——它可以工作，并且是通用的，但是效率不高。

```
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int q = sc.nextInt();
        if (n < 1 || q < 1) {
            sc.close();
            return;
        }

        /// HashMap solution
        Map<Integer, Set<Integer>> hash = new HashMap<>();
        while (q-- > 0) {
            String c = sc.next();
            if (c.equals("M")) {
                int a = sc.nextInt();
                int b = sc.nextInt();  
                Set<Integer> seta = null;
                Set<Integer> setb = null;
                if (hash.containsKey(a)) {
                    seta = hash.get(a);    
                }
                if (hash.containsKey(b)) {
                    setb = hash.get(b);    
                }     
                if (seta == null && setb == null) {
                    seta = new HashSet<Integer>();
                    seta.add(a);
                    seta.add(b);
                    hash.put(a, seta);
                    hash.put(b, seta);
                }
                else if (seta == null) {
                    setb.add(a);
                    hash.put(a, setb);    
                }
                else if (setb == null) {
                    seta.add(b);
                    hash.put(b, seta);
                }
                else {
                    seta.addAll(setb);
                    for (Integer i : setb) {
                        hash.put(i, seta);
                    }
                }     
            }
            else if (c.equals("Q")) {
                int i = sc.nextInt();
                if (hash.containsKey(i)) {
                    System.out.println(hash.get(i).size());
                }
                else {
                    System.out.println(1);    
                }
            }
        }

    }
}
```

**版本 2** :针对这个问题使用树合并的方法进行了优化。

```
import java.io.*;
import java.util.*;

class Node {
    int parent = -1;
    int num = 1;
    static int findRoot(Node [] tree, int i) {
        // assume it's the root, or fix it
        int root = i;
        while (tree[root].parent != -1) {
            root = tree[root].parent;    
        }

        // flat the tree
        if (root != i) {
            tree[i].parent = root;
            // further optimize
            // int j = i;
            // while (j != root) {
            //     int p = tree[j].parent;
            //     tree[i].parent = root;
            //     j = p;
            // }
        }

        return root; 
    }
}

public class Solution {

    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int q = sc.nextInt();
        if (n < 1 || q < 1) {
            sc.close();
            return;
        }

        // Tree solution
        Node [] tree = new Node[n+1];
        for (int i = 1; i < tree.length; i++) {
            tree[i] = new Node();
        }
        while (q-- > 0) {
            String c = sc.next();
            if (c.equals("M")) {
                int a = sc.nextInt();
                int b = sc.nextInt();
                int ra = Node.findRoot(tree, a);
                int rb = Node.findRoot(tree, b);
                if (ra != rb) {
                    tree[ra].parent = rb;
                    tree[rb].num += tree[ra].num;
                }
            }
            else if (c.equals("Q")) {
                int i = sc.nextInt();
                int ri = Node.findRoot(tree, i);
                System.out.println(tree[ri].num);
            }
        }
        sc.close();

    }
}
```

因此树合并是解决这种合并问题的有效方法。

编码快乐！

*问题，想法？在这里留下评论。跟随我成为有趣的解决问题之旅的一部分。*