# HackerRank 无前缀集解决方案

> 原文：<https://medium.com/geekculture/hackerrank-no-prefix-set-solution-6ddd99a7d00?source=collection_archive---------2----------------------->

## 解决 HackerRank 无前缀集问题

![](img/4f24781941365fb966e5dbd89a9c4d6e.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 问题

有一个给定的字符串列表，其中每个字符串只包含从 a 到 j 的小写字母。如果没有一个字符串是另一个字符串的前缀，则称该字符串集为**好集**。在这种情况下，打印**设置好**。否则，在第一行打印**坏集**，后面跟着被检查的字符串。

**注意**如果两个字符串相同，则它们是彼此的前缀。

**例子**

```
words = ['abcd', 'bcd', 'adcde', 'bcde']
```

这里' abcd '是' abcde '的前缀，' bcd '是' bcde '的前缀。因为首先测试“abcde ”,所以打印

```
BAD SET  
abcde
```

```
words = ['ab', 'bc', 'cd']
```

没有字符串是另一个的前缀，所以打印

```
GOOD SET
```

黑客排名的问题在于

[](https://www.hackerrank.com/challenges/no-prefix-set/problem) [## 无前缀集| HackerRank

### 有一个给定的字符串列表，其中每个字符串只包含小写字母。那套琴弦…

www.hackerrank.com](https://www.hackerrank.com/challenges/no-prefix-set/problem) 

## 解决方法

这是一个很难解决的黑客问题。让我们一步一步去征服它。下面将提供 3 个解决方案，从容易理解到优化良好。

**解决方案 1:简单易懂的强力解决方案**

```
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

class Result {

    /*
     * Complete the 'noPrefix' function below.
     *
     * The function accepts STRING_ARRAY words as parameter.
     */

    public static void noPrefix(List<String> words) {
        // Write your code here
        if (words == null || words.size() < 2) {
            System.out.println("GOOD SET");
            return;
        }

        // brutal force
        for (int i = 0; i < words.size(); i++) {
            for (int j = 0; j < words.size(); j++) {
                if (i != j && 
                    words.get(i).startsWith(words.get(j))) {
                    System.out.println("BAD SET");
                    System.out.println(words.get(i));
                    return;
                }    
            }
        }

        System.out.println("GOOD SET");
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<String> words = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            String wordsItem = bufferedReader.readLine();
            words.add(wordsItem);
        }

        Result.noPrefix(words);

        bufferedReader.close();
    }
}
```

上面的解决方案是 O(n)并且没有优化，但是在一些用例中会失败。

**解决方案 2:使用 Trie 和通用 HashMap 解决方案**

这是一个与 Trie 相关的问题。让我们使用一个通用实现的 Trie 来解决它。

```
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

class Result {

    /*
     * Complete the 'noPrefix' function below.
     *
     * The function accepts STRING_ARRAY words as parameter.
     */

    public static void noPrefix(List<String> words) {
        // Write your code here
        if (words == null || words.size() < 2) {
            System.out.println("GOOD SET");
            return;
        }

        for (int i = 0; i < words.size(); i++) {
            Map<Character, TrieNode> children = root.children;
            for (char ch : words.get(i).toCharArray()) {
                if (children.containsKey(Character.valueOf('*'))) {
                    System.out.println("BAD SET");
                    System.out.println(words.get(i));
                    return;
                }
                if (children.containsKey(Character.valueOf(ch))) {
                    children = children.get(Character.valueOf(ch)).children;
                }
                else {
                    TrieNode gchild = new TrieNode();
                    gchild.first = words.get(i);
                    children.put(Character.valueOf(ch), gchild);
                    children = gchild.children;
                }
            }
            if (children.size() > 1) {
                System.out.println("BAD SET");
                if (children.containsKey(Character.valueOf('*'))) {
                    System.out.println(words.get(i));
                }
                else {
                    System.out.println(children.get(Character.valueOf('1')).first);
                }
                return;    
            }
            children.put(Character.valueOf('*'), new TrieNode());
        }

        System.out.println("GOOD SET");
    }

    static TrieNode root = new TrieNode();

    static class TrieNode {
        String first;
        Map<Character, TrieNode> children = new HashMap<>();
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<String> words = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            String wordsItem = bufferedReader.readLine();
            words.add(wordsItem);
        }

        Result.noPrefix(words);

        bufferedReader.close();
    }
}
```

这是使用 HashMap 实现通用 Trie 的一个很好的练习，尽管它并没有针对这个特定于 HackerRank 的问题进行优化。现在还是 O(n)。

**解决方案 3:为问题设计一个定制的 Trie**

现在，我们可以使用相同的 Trie 思想来优化这个特定的问题。

```
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

class Trie {
    private static final int LETTER_SIZE = 'j' - 'a' + 1;

    class Node {

        Node[] children;
        char key;
        int wordCount = 0;
        int prefixCount = 0;

        Node(char key) {
            this.key = key;
            this.children = new Node[LETTER_SIZE];
        }

        boolean has(char key) {
            return get(key) != null;
        }

        Node get(char key) {
            return children[getKey(key)];
        }

        void put(char key, Node node) {
            children[getKey(key)] = node;
        }

        char getKey(char ch) {
            return (char) (ch - 'a');
        }
    }

    private Node root = new Node('*');

    public boolean insert(String word) {
        return insert(word, root);
    }

    private boolean insert(String word, Node parent) {
        parent.prefixCount++;
        if (word.length() >= 0 && parent.wordCount > 0) {
            return false;
        }
        if (word.length() == 0) {
            if (parent.prefixCount > 1) {
                return false;
            }
            parent.wordCount++;            
            return true;
        }        

        char ch = word.charAt(0);
        Node next = parent.get(ch);        
        if (next == null) {
            next = new Node(ch);            
            parent.put(ch, next);
        }        
        return insert(word.substring(1), next);        
    }    
}

class Result {

    /*
     * Complete the 'noPrefix' function below.
     *
     * The function accepts STRING_ARRAY words as parameter.
     */

    public static void noPrefix(List<String> words) {
        // Write your code here

        boolean good = true;
        Trie trie = new Trie();
        for (String word : words) {
            good = trie.insert(word);            
            if (!good) {
                System.out.println("BAD SET");  
                System.out.println(word);  
                break;
            }
        }

        if (good) {
            System.out.println("GOOD SET");
        }  
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<String> words = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            String wordsItem = bufferedReader.readLine();
            words.add(wordsItem);
        }

        Result.noPrefix(words);

        bufferedReader.close();
    }
}
```

解决方案可以 100%被 HackerRank 接受。它是 O(nlogn ),因为每个 Trie 操作都需要 logn。

现在我们简化一下，只针对面试问题。

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
     * Complete the 'noPrefix' function below.
     *
     * The function accepts STRING_ARRAY words as parameter.
     */
    public static void noPrefix(List<String> words) {
        // Write your code here
        Trie trie = new Trie();
        boolean good = true;
        for (String word : words) {
            if (trie.insert(word)) {
                System.out.println("BAD SET");
                System.out.println(word);
                good = false;
                break;
            }
        }
        if (good)
            System.out.println("GOOD SET");
    }
}

class Trie {
    int LEN = 'j' - 'a' + 1;
    Node root = new Node();

    class Node {
        Node [] children;
        boolean wordleaf;

        Node() {
            children = new Node[LEN];
            wordleaf = false;
        }
    }

    // return true if prefix, otherwise false
    boolean insert(String str) {
        return insert(str, root);    
    }

    boolean insert(String str, Node node) {
        if (str == null || str.length() < 1) return false;
        char c = str.charAt(0);
        Node child = node.children[c - 'a'];
        if (child == null) {
            // extend
            child = new Node();
            node.children[c - 'a'] = child;
        }
        // support both prefix cases, such as (a, ab) and (ab, a)
        else if (child.wordleaf || str.length() == 1) {
            return true;
        }

        if (str.length() == 1) {
            child.wordleaf = true;
            return false;    
        }
        return insert(str.substring(1), child);
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<String> words = IntStream.range(0, n).mapToObj(i -> {
            try {
                return bufferedReader.readLine();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .collect(toList());

        Result.noPrefix(words);

        bufferedReader.close();
    }
}
```

上面也 100%通过了所有的 HackerRank 测试案例，但它只是针对这个问题进行了简化。

编码快乐！

*问题、想法？请在下面的回复中留下评论。如果你喜欢解决棘手问题的旅程，请联系我。*