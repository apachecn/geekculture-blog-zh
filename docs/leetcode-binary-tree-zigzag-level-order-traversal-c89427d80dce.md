# LeetCode —二叉树之字形层次顺序遍历

> 原文：<https://medium.com/geekculture/leetcode-binary-tree-zigzag-level-order-traversal-c89427d80dce?source=collection_archive---------23----------------------->

# 问题陈述

给定二叉树的`root`,返回*其节点值的锯齿形层次顺序遍历。*(即从左到右，然后从右到左进行下一级并交替进行)。

问题陈述摘自:[https://leet code . com/problems/binary-tree-zigzag-level-order-traversal/](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

**例 1:**

![](img/d31af91908fa4bd9258b09296f295df8.png)

LeetCode

```
Input: root = [3, 9, 20, null, null, 15, 7]
Output: [[3], [20, 9], [15, 7]]
```

**例 2:**

```
Input: root = [1]
Output: [[1]]
```

**例 3:**

```
Input: root = []
Output: []
```

**约束:**

```
- The number of nodes in the tree is in the range [0, 2000].
- -100 <= Node.val <= 100
```

# 说明

要逐层遍历二叉树，我们可以参考我们之前的博文[这里](https://alkeshghorpade.me/post/leetcode-binary-tree-level-order-traversal)。

根据问题陈述，我们需要以之字形方式穿越。我们可以通过在完全遍历一个级别时反转我们创建的`tmp`数组来实现这一点。

让我们检查算法:

```
- initialize 2D array as vector vector<vector<int>> result
- initialize size and i
- set left to true

- return result if root == null

- initialize queue<TreeNode*> q
  - push root to queue : q.push(root)

- initialize TreeNode* node for iterating on the tree

- loop while( !q.empty() ) // queue is not empty
  - initialize vector<int> tmp
  - set size = q.size()

  - loop for i = 0; i < size; i++
    - set node = q.front()

    - if node->left
      - push in queue: q.push(node->left)

    - if node->right
      - push in queue: q.push(node->right)

    - remove the front node: q.pop()
    - push node->val to tmp. tmp.push_back(node->val)

  - left is false: if(!left)
    - reverse(tmp.begin(), tmp.end())

  - push the tmp to result: result.push_back(tmp)
  - toggle left: left = !left

- return result
```

## C++解决方案

```
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int size, i;
        bool left = true;

        if(root == NULL)
            return result;

        queue<TreeNode* > q;
        q.push(root);

        TreeNode* node;

        while(!q.empty()){
            vector<int> tmp;
            size = q.size();

            for(i = 0; i < size; i++){
                node = q.front();

                if(node->left)
                    q.push(node->left);

                if(node->right)
                    q.push(node->right);

                q.pop();
                tmp.push_back(node->val);
            }

            if(!left){
                reverse(tmp.begin(), tmp.end());
            }

            result.push_back(tmp);
            left = !left;
        }

        return result;
    }
};
```

## 戈朗溶液

```
func reverse(array []int) []int {
    for i, j := 0, len(array) - 1; i < j; i, j = i+1, j-1 {
        array[i], array[j] = array[j], array[i]
    }

    return array
}

func zigzagLevelOrder(root *TreeNode) [][]int {
    result := [][]int{}
    left := true

    queue := []*TreeNode{root}

    for len(queue) != 0 {
        tmp := []int{}
        size := len(queue)

        for i := 0; i < size; i++ {
            if queue[0] != nil {
                tmp = append(tmp, queue[0].Val)
                queue = append(queue, queue[0].Left)
                queue = append(queue, queue[0].Right)
            }

            queue = queue[1:]
        }

        if !left {
            tmp = reverse(tmp)
        }

        result = append(result, tmp)
        left = !left
    }

    return result[:len(result)-1]
}
```

## Javascript 解决方案

```
var zigzagLevelOrder = function(root) {
    let result = [];
    let queue = [];
    let left = true;

    if(root)
        queue.push(root);

    while(queue.length > 0) {
        tmp = [];
        let len = queue.length;

        for (let i = 0; i< len; i++) {
            let node = queue.shift();
            tmp.push(node.val);

            if(node.left) {
                queue.push(node.left);
            }

            if(node.right) {
                queue.push(node.right);
            }
        }

        if( !left ) {
            tmp = tmp.reverse();
        }

        result.push(tmp);
        left = !left;
    }

    return result;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: root = [3, 9, 20, null, null, 15, 7]

Step 1: vector<vector<int>> result
        int size, i
        left = true

Step 2: root == null
        [3, 9..] == null
        false

Step 3: queue<TreeNode*> q
        q.push(root)

        q = [3]

Step 4: loop !q.empty()
        q = [3]
        q.empty() = false
        !false = true

        vector<int> tmp
        size = q.size()
             = 1

        for(i = 0; i < 1; i++)
          - 0 < 1
          - true

          node = q.front()
          node = 3

          if node->left
            - node->left = 9
            - q.push(node->left)
            - q = [3, 9]

          if node->right
            - node->right = 20
            - q.push(node->right)
            - q = [3, 9, 20]

          q.pop()
          q = [9, 20]

          tmp.push_back(node->val)
          tmp.push_back(3)

          i++
          i = 1

        for(i < 1)
        1 < 1
        false

        if !left
           !left = false

        result.push_back(tmp)
        result = [[3]]
        left = !left
             = false

Step 5: loop !q.empty()
        q = [9, 20]
        q.empty() = false
        !false = true

        vector<int> tmp
        size = q.size()
             = 2

        for(i = 0; i < 2; i++)
          - 0 < 2
          - true

          node = q.front()
          node = 9

          if node->left
            - node->left = nil
            - false

          if node->right
            - node->right = nil
            - false

          q.pop()
          q = [20]

          tmp.push_back(node->val)
          tmp.push_back(9)

          i++
          i = 1

        for(i < 2)
          - 1 < 2
          - true

          node = q.front()
          node = 20

          if node->left
            - node->left = 15
            - q.push(node->left)
            - q = [20, 15]

          if node->right
            - node->left = 7
            - q.push(node->right)
            - q = [20, 15, 7]

          q.pop()
          q = [15, 7]

          tmp.push_back(node->val)
          tmp.push_back(20)
          tmp = [9, 20]

          i++
          i = 2

        for(i < 2)
          - 2 < 2
          - false

        if !left
           !left = true

        reverse(tmp.begin(), tmp.end())
        tmp = [20, 9]

        result.push_back(tmp)
        result = [[3], [20, 9]]
        left = !left
             = true

Step 6: loop !q.empty()
        q = [15, 7]
        q.empty() = false
        !false = true

        vector<int> tmp
        size = q.size()
             = 2

        for(i = 0; i < 2; i++)
          - 0 < 2
          - true

          node = q.front()
          node = 15

          if node->left
            - node->left = nil
            - false

          if node->right
            - node->right = nil
            - false

          q.pop()
          q = [7]

          tmp.push_back(node->val)
          tmp.push_back(15)

          i++
          i = 1

        for(i < 2)
          - 1 < 2
          - true

          node = q.front()
          node = 7

          if node->left
            - node->left = nil
            - false

          if node->right
            - node->right = nil
            - false

          q.pop()
          q = []

          tmp.push_back(node->val)
          tmp.push_back(7)
          tmp = [15, 7]

          i++
          i = 2

        for(i < 2)
          - 2 < 2
          - false

        if !left
           !left = false

        result.push_back(tmp)
        result = [[3], [20, 9], [15, 7]]

Step 7: loop !q.empty()
        q = []
        q.empty() = true
        !true = false

Step 8: return result

So we return the result as [[3], [20, 9], [15, 7]].
```

*最初发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-binary-tree-zigzag-level-order-traversal)*。*