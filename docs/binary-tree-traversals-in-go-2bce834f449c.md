# Go 中的二叉树遍历

> 原文：<https://medium.com/geekculture/binary-tree-traversals-in-go-2bce834f449c?source=collection_archive---------21----------------------->

当我第一次开始我的围棋之旅时，我想挑选几个简单直观的问题，并在 Go 中编写它们的解决方案。这不仅帮助我掌握了语言语法，理解了特定于 Go 的编码概念，而且对于开发将算法从描述形式转化为 Go 中的工作解决方案所需的技能也是至关重要的。

在《数据结构与算法》课程中，可以发现很多这样的问题。Leetcode 等网站有数百个问题，不仅测试你的算法技能，也测试你的编码技能。在本文中，我将展示如何在 Go 中执行二叉树遍历，从定义表示树节点的必要数据结构开始。我还将展示执行这种遍历的不同方式——使用递归、使用函数闭包和使用迭代。

这是本系列的第 1 部分——在 Go 中使用二叉树。

*   第 1 部分 Go 中的二叉树遍历(本文)
*   第 2 部分在 Go 中遍历二叉树——使用迭代(在这里访问它
*   第三部分 Go 中二叉树的层次顺序遍历(此处访问)
*   第四部分 Go 中二叉树的曲折层次顺序遍历(此处访问)
*   第 5 部分 Go 中二叉树的右侧视图
*   第 6 部分 Go 中的二叉树序列化

二叉树 T 被递归地定义。它被定义为空的或这样的

*   它有一个特殊的节点叫做根节点。
*   t 有两组节点，组织成左子树和右子树
*   左子树和右子树本身都是二叉树

二叉树中的每个节点最多有两个子节点，这两个子节点都可以是空的或非空的。我们如何在围棋中表现这样一个节点？它相当简单，如下所示。

Representing a Binary Tree Node

**二叉树遍历**

在检查或操作树结构时，遍历二叉树是一个常见的场景。我们可以在遍历二叉树的过程中做出选择，也就是说，我们可以选择是先访问根还是先访问子树。这样的选择会导致不同的遍历顺序。下面的三个顺序导致二叉树的三种不同的遍历。

**顺序遍历**

*   首先访问或遍历左边的子树
*   接下来，访问根节点
*   最后，访问右边的子树

**前序遍历**

*   首先访问根节点
*   接下来，访问左边的子树
*   最后，访问右边的子树

**后序遍历**

*   首先访问左边的子树
*   接下来，访问右边的子树
*   最后，访问根节点

如你所见，定义非常简单，递归使得遍历算法的编码变得容易。在下面的代码中，我以有序的方式打印所有的节点值。

Inorder using recursion

我还想展示如何使用函数闭包来收集切片中的所有节点值，并将其返回给父函数。Go 中的函数闭包是一个绑定到其作用域之外的变量的函数，即它引用了其主体之外的一个(或多个)变量。下面的代码片段正好说明了这一点。

Top-level inorder function that uses a function closure to process each node

InorderInternal function that recursively calls itself

前序和后序函数可以类似地编码。您将注意到的唯一变化是访问节点的顺序。

**使用递归进行前序遍历**

Preorder traversal using Recursion

**使用递归和函数闭包的前序遍历**

Top-level preorder function that uses a function closure

A preorder internal function that calls itself

**使用递归进行后置处理**

Postorder traversal using recursion

**使用递归和函数闭包进行后置排序**

Top-level postorder function that uses a closure

A postorder internal function that calls itself

我将在本系列的第 2 部分展示如何迭代地实现这些遍历算法。