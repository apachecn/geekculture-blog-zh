# 关于数据结构的面试问题

> 原文：<https://medium.com/geekculture/interview-questions-on-data-structures-417761216620?source=collection_archive---------7----------------------->

## 面试经验和指南—第四部分

## 软件工程数据结构面试准备

*大家好*😊*，今天我将分享一些常见的数据结构相关的面试问题，作为我的软件工程师面试准备系列的一部分。这是该系列的第四部分。万一错过了前几期，可以在这里阅读第一部分*[](/geekculture/interview-preparation-kid-for-software-engineer-1380f6fcbae9)**[*OOP&Java*](/javarevisited/interview-questions-on-object-oriented-programming-and-java-41b027d93ddb)*[*数据库*](https://faun.pub/interview-questions-on-database-concepts-d480defce050) *。不再拖延，让我们进入今天的主题数据结构。****

**![](img/4cfef48e7ff4ef0ad9c580969a16de1a.png)**

**Photo by [Edgar Chaparro](https://unsplash.com/@echaparro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**数据结构是一种在计算机中组织数据的方法，以便可以有效地使用它。每种编程语言中都有很多数据结构。每一种都有自己的特点和优缺点。由于数据结构是软件工程的基础知识，对于软件工程师来说，将会有大量的数据结构面试问题。为了开发任何类型的应用程序或任何东西，一个或多个数据结构的存在在软件开发中是不可避免的。它被用于从 **Hello World** 程序到软件行业的复杂问题。在本文中，介绍了 Java 中可用的主要数据结构。让我们转到数据结构中的访谈观点。这些是面试中被问得最多的问题。**

1.  **什么是线性和非线性数据结构？请给出一些例子。**

*   **线性数据结构是这样一种结构，其中数据项被连续或线性地存储，每个成员被附加到它的前一个和下一个相邻元素上。因此，我们可以在一次运行中探索所有的元素。因为计算机内存是以线性方式组织的，所以线性数据结构很容易构建。例如:列表，堆栈，队列，数组列表**
*   **非线性数据结构是数据元素没有顺序或线性排序的数据结构。因此，我们无法在一次运行中遍历所有的元素。与线性数据结构相比，非线性数据结构更难构建。树木、地图、图表。**

**![](img/a19636b6ee2104688422b8f7aece50a6.png)**

**Photo by [Denny Müller](https://unsplash.com/@redaquamedia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**2.你认为链表是线性数据结构吗？**

*   **根据应用的不同，链表可以分为线性或非线性数据结构。当用于访问方法时，它是一种线性数据结构。当用于数据存储时，它是一种非线性数据结构。**

**3.**Java 中 Array 和 LinkedList 的比较对比？****

*   **数组:连续内存分配，大小固定，可以通过索引访问，中间的插入和删除开销很大。**
*   **LinkedList:不需要连续内存分配，动态内存分配(无固定大小)，不需要索引访问，中间遍历集合进行访问、插入和删除，成本相对低廉，指针额外的内存成本。**

**4.**堆栈和队列数据结构有什么区别？****

*   **堆栈是一种线性数据结构，其中的元素只能从数据结构的顶部添加和删除。后进先出法后面是堆栈(后进先出)。将一个元素插入到堆栈中称为压入操作，从堆栈中移除一个元素称为弹出操作。**
*   **下面是堆栈的一些应用。检查表达式中的平衡括号，计算后缀表达式，反转字符串**
*   **队列是一种线性数据结构，其中的元素只能从列表的一侧插入，称为尾部，并且只能从另一侧删除，称为前端。在队列中使用 FIFO(先进先出)方法来访问元素。这里，en queue 用于插入，dequeue 用于删除。**
*   **只有一个指针，称为顶部，用于访问堆栈中的列表，它总是指向列表中的最后一个条目。我们在队列中保存两个指针来访问列表。第一个指针始终存在并指向列表中的第一个元素，而后面的指针始终存在并指向最后插入的元素。**
*   **队列的一些应用有:网站请求处理、CPU 任务调度、在 MP3 媒体播放器、CD 播放器等应用中用作缓冲区。**

**![](img/6002f276a9971b10b2ee24230f32dead.png)**

**Photo by [Zichao Zhang](https://unsplash.com/@shakusky?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**5.**图和树的数据结构有什么区别？****

*   **图:非线性数据结构，顶点/节点和边的集合，节点间多条路径，没有唯一的节点叫根，可以形成一个循环。应用:在网络中寻找最短路径**
*   **树:非线性数据结构，顶点/节点和边的集合，从一个节点到另一个节点只有一条路径，一般的树由具有任意数量子节点的节点组成。但是在二叉树的情况下，每个节点最多可以有两个子节点。有一个唯一的节点叫根，不会有循环。应用:博弈树，决策树。**

**6.**数组和集合有什么区别？****

*   **数组和集合的最大区别是数组可以有重复的值，而集合不能有重复的值。**
*   **数组中的数据按照索引排序，其中资产使用键&元素按照插入顺序是可迭代的。**

**7.什么是动态数据结构？**

*   **它们是数据的内存集合，在程序运行时可以扩展和收缩，允许它们增加或减少大小。这允许程序员精确地管理使用的内存量。**
*   **动态数组、链表、栈、队列和堆是数据结构的一些例子。**

**8.【hashmap 和 hashtable 有什么区别？**

*   **Hashmap 不同步。不能在多线程环境中使用。但是 Hashtable 是同步的。**
*   **Hashmap 允许一个空键和多个空值，而 hashtable 不允许任何空键或空值。**
*   **如果不需要线程安全，Hashmap 通常比 hashtable 更受青睐。**

**![](img/e47da9f5bc7e3e4a9d7e3c23894470e6.png)**

**Photo by [Masha Kotliarenko](https://unsplash.com/@kotliarenko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**9.**什么是双向链表？****

*   **双向链表是一种更复杂的链表，其中每个节点都有一个指针指向序列中的上一个和下一个节点。双向链表中的一个节点由三部分组成:数据、下一个数据指针和前一个数据指针。**
*   **双向链表可以用于软件中的撤销重做操作，音乐播放器中的下一首歌，上一首歌等。**

**10.**什么是出列？****

*   **出列(双端队列)是一组有序的元素，可以在两端插入和删除。**

**11.**Java 中 HashMap 如何处理冲突？****

*   **为了解决冲突，Java 的 *Java.util.HashMap* 类使用了链接机制。如果尝试推送具有相同键的新值，这些值将保存在一个链表中，该链表作为一个链与链接中的现有值一起存储在键的桶中。**
*   **在最坏的情况下，所有的键可能有相同的 hashcode，导致哈希表被转换成一个链表。由于链表的结构，搜索一个值将需要 O(n)时间，而不是本例中的 O(1)时间。因此，在选择哈希算法时必须小心谨慎。**

**![](img/23ddd8c17a33898316bf48a747835e4e.png)**

**Photo by [Julie Tupas](https://unsplash.com/@jltps?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**12.**什么是堆数据结构？****

*   **Heap 是一种建立在树上的非线性数据结构，以一棵完整的二叉树为树。如果除了最后一层之外的所有层都被完全填充，则二叉树被认为是满的，最后一层的所有组件都尽可能地指向左边。堆有两种类型。**
*   **Max-Heap:在 Max-Heap 中，根节点的数据元素必须是树中所有数据项中最重要的。这个条件应该递归地适用于二叉树所有子树。**
*   **最小堆:在最小堆中，根节点的数据元素必须是树中所有数据项中最小的。这个条件应该递归地适用于二叉树所有子树。**

**![](img/8f19f0501ab9dbb93a7852c1c5074978.png)**

**Photo by [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**13.**如何在给定的未排序数组中找到第二大的数？****

**14.**如何在不使用第三个变量的情况下交换两个整数？****

```
**int a=15;
int b=12;
a=a+b
b=a-b
a=a-b
// Now values of a,b are exchanged without using a third variable**
```

**15.**在给定字符串中寻找第一个重复字母的算法？****

*   **这里给定算法的时间复杂度是 O(n)。HashSet 用于获得在集合中搜索的时间复杂度为 O(1)。**
*   **Break 用于避免在找到副本后运行 for 循环。**

**16.**用堆代替栈有什么好处？****

*   **因为内存空间可以根据需要动态分配和释放，所以堆比栈更灵活。在 Java 中，对象存储在堆内存中，而局部变量和函数调用存储在堆栈内存中。**
*   **保存在堆栈中的变量只能由用户作为私有内存访问，但是堆中产生的对象对所有线程都是可见的。使用递归时，堆内存的大小会增加，而堆栈内存会很快填满。**

**![](img/d3bfa573f349847562ba279b22c150f0.png)**

**Photo by [Bekir Dönmez](https://unsplash.com/@bekirdonmeez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**17.**二叉树可以有多少个节点有限制吗？****

*   **当节点的值为空时，二叉树可能至少有零个节点。此外，二叉树可以有一个或两个节点。**

**18.**堆栈和数组**有什么区别？**

*   **后进先出模式后面是堆栈。它表示数据是按顺序访问的，最近存储的数据首先被提取。另一方面，数组不遵循任何顺序，可以简单地通过引用数组的索引元素来访问。**

**19.**在链表中，如何寻找某个键？****

*   **顺序搜索必须用于在链表中查找目标键。每个节点被访问并与目标关键字进行比较；如果两者不同，它将沿着链接前进到下一个节点。这个过程一直持续到找到目标键或最后一个节点。**

**![](img/dede871b1573ab86884bd15e8c59a54a.png)**

**xPhoto by [Karine Avetisyan](https://unsplash.com/@kar111?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**

**20.**什么是 AVL 树？****

*   **AVL 树是自平衡二叉查找树(BST ),所有节点的左右子树之间的最大高度差为 1。**

**21.**最适合用来检查一个字符串是否为回文的数据结构是什么？****

*   **堆**
*   **确定字符串的长度。现在你必须能够找到中间。
    所有的项目都应该被推到堆栈的中间。
    如果字符串的长度是奇数，忽略中间的字符。不断从堆栈中弹出项目，并将它们与当前字符进行比较，直到字符串结束。**
*   **如果不匹配，字符串就不是回文。如果所有的部分都匹配，这个字符串就是一个回文。**

**我相信你已经理解了上面讨论的与面试的数据结构主题相关的所有问题。如果您有任何问题或任何澄清，不要犹豫，通过回复部分与我联系。感谢你花宝贵的时间阅读这篇博客，我相信这会激励你继续阅读其他关于面对面试的博客。**

**喜欢这篇文章吗？成为 [*中等会员*](https://sthenusan.medium.com/membership) *继续无限制学习。如果你使用上面的链接，我会收到你的一部分会员费，不需要你额外付费。提前感谢。***

**![](img/c20766cf7105311ca13cd46e6ae0fd2d.png)**

**Photo by [Alexas_Fotos](https://unsplash.com/@alexas_fotos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**