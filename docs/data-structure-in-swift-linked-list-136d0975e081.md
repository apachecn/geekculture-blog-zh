# Swift 中的数据结构:链表

> 原文：<https://medium.com/geekculture/data-structure-in-swift-linked-list-136d0975e081?source=collection_archive---------10----------------------->

## 在本文中，我想总结一下我使用 Swift 学习链表数据结构的经验。

![](img/4228c0c4f251bcc7800e90fcd530107d.png)

Photo by [Nate Grant](https://unsplash.com/@nateggrant?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 概观

链表基本上是对象的集合，其中每个对象(通常称为节点)都有对下一个节点的引用。你可以把链表想象成一条链。下面是链表表示法:

![](img/b17d3a616e70250602a86d9ddb301e6a.png)

Source: [https://www.geeksforgeeks.org/data-structures/linked-list/](https://www.geeksforgeeks.org/data-structures/linked-list/)

节点对象保存数据和对下一个节点的引用。第一个节点叫`Head`，最后一个节点叫`Tail`。在上图中，它通常被称为常规链表。还有一些其他类型的链表，如双向链表，其中每个节点保存下一个引用和上一个引用，以及循环链表，其中从尾部开始的下一个引用是头部。

下面是用 Swift 编写的节点对象实现:

我们可以使用通用实现，这样节点对象可以保存任何数据类型。这个类也符合`CustomStringConvertible`，只是为了让我们通过获取它的描述来打印链表数据变得更容易。

下面是用 Swift 编写的链表实现:

在链表类中，它有两个可选属性，分别是`head`和`tail`。头部是列表中的第一个节点，尾部是列表中的最后一个节点。同样，为了让我们的生活变得更容易，我扩展了链表类以符合`CustomStringConvertible`协议。我们可以通过调用`description`值来打印列表。

最后，我们已经创建了节点和链表类，让我们进行一些测试:

```
**let** node = Node<Int>(value: 0)**let** node2 = Node<Int>(value: 1)**let** node3 = Node<Int>(value: 2)node.next = node2node2.next = node3 **let** ll = LinkedList<Int>(head: node)// result
**Optional(0 -> 1 -> 2)**print(ll) 
```

恭喜你，你已经知道如何从链表数据结构创建基本功能。让我们通过添加一些功能来改进这个实现。

## 推送操作

推送操作是在前端添加新节点——或者换句话说，我们添加新节点并使其成为新的头。这个操作的复杂度应该是 O(1 ),因为我们只需要知道当前的`head`是否存在。如果当前的`head`为空，那么我们可以直接插入新的节点并使其成为新的`head`，如果当前的`head`存在，那么我们可以创建新的节点并将引用传递给旧的`head`，并使新的节点成为新的`head`。

```
**let** ll = LinkedList<Int>()ll.push(value: 3)ll.push(value: 2)ll.push(value: 1)print(ll) // **Optional(1 -> 2 -> 3)**
```

## 追加操作

另一个操作是追加。追加是在列表的最后添加新节点并使其成为新的`tail`的操作。它的复杂度为 O(n ),因为它需要迭代到列表的末尾。

```
**let** ll = LinkedList<Int>()ll.append(value: 1)ll.append(value: 2)ll.append(value: 3)print(ll) // 1 -> 2 -> 3
```

第一步是我们需要检查链表是否为空。如果列表是空的，那么我们可以调用之前已经创建的 push 函数。如果列表不为空，那么我们可以在第一时间创建一些变量来保存列表的`head`。然后我们可以使用 While 循环迭代节点，条件是`current`不为零。在循环内部，我们检查`next`是否不为零，然后我们用`next`引用赋值`current`。如果`next`为零，那么我们知道`current`是一个`tail`，所以我们可以将`current`分配给`newNode`的下一个，并将其作为一个新的`tail`。

## 操作时插入

插入操作是在给定的索引处添加新的节点。实现可能类似于追加操作，我们迭代，直到我们满足给定的索引。我们可以创建一个变量作为计数器，并检查计数器是否与给定的索引相同，然后将节点插入到那里。

这次的实现有点复杂。但是让我们分解代码:

1.  第一次，我们需要检查索引是否大于 0，列表是否不为空。如果索引为 0 或者列表为空，那么我们可以调用之前已经创建的 push 函数
2.  就像 append 操作一样，我们创建变量来保存 head 作为第一个节点。此时，我们还创建了`counter`变量和`prev`来保存循环开始时的前一个节点。稍后，当我们已经插入新节点时，需要使用`prev`变量来设置新的引用。
3.  在循环内部，我们检查`index`是否与`counter`不同。如果`index`和`counter`已经相同，那么我们设置`current`节点旁边的`newNode`，下一个`prev`设置为`newNode`。
4.  该代码检查当前`next`是否为零，但给定的索引与计数器不一致。你可以想象一下，如果我们的链表中只有 3 个数据，而给定的索引是 5。在这种情况下，我们只需要像 append 操作一样添加新节点作为尾部。
5.  如果给定的索引仍然与计数器不相同，并且当前的 next 不为零，则循环必须继续。我们用`current`指定`prev`值，用`next`参考指定`current`，并递增`counter`。

## 挑战

是的，您已经实现了一些重要的功能，如 push、append 和 insert at 操作。让我们给你一些挑战作为家庭作业。

1.  **在操作中删除**
2.  **获取中间节点**
3.  **反向链表**

如果你想认真学习链表，我鼓励你接受挑战，自己尝试一下。如果你卡住了，那么你可以回到这里，看看我的实现。

## 在解决方案时删除

## 获取中间节点解决方案

## 反向链表解决方案

## 结论

祝贺您关注这篇文章。你是知道链表的高手:】。你可能不会在日常项目中使用链表，但是作为一名软件工程师，知道这一点对你是有好处的。我可能会在另一个时间写另一个数据结构。如果你对这篇文章满意，请鼓掌并给我一些反馈:】。