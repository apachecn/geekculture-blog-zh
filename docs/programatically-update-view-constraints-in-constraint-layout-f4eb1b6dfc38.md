# 以编程方式更新约束布局中的视图约束

> 原文：<https://medium.com/geekculture/programatically-update-view-constraints-in-constraint-layout-f4eb1b6dfc38?source=collection_archive---------13----------------------->

![](img/b52794d4d820f1a0c19cf8ae6818a8a2.png)

Constraint layout 是迄今为止 Android 框架中最通用的视图组之一，然而，以编程方式更新视图约束所需的 Kotlin 代码可能有点冗长。我已经创建了一个约束布局 Kotlin 扩展，它将使这个过程更容易应用，在代码中看起来更清晰。我们开始吧！

# 约束说明

这里我们有 **ConstraintInstructions** 接口，它有两个简单的实现， **ConnectConstraint** 和 **DisconnectConstraint** 。

**ConnectConstraints** 采用 4 个参数来帮助描述您要约束的视图以及约束的内容。

1.  startID —您要约束的视图的 resourceId。
2.  startSide —作为 startID 传递的视图中您想要约束的一侧。传入 **side* 参数的值应该是 ConstraintSet 类中的常量。常见的有:ConstraintSet。*开始，*约束集。*顶部，*约束集。*结束，或*约束集。*底部。*
3.  endID —您希望将**约束到**的视图的 resourceId。请记住，通常视图被约束到另一个视图或父视图，所以如果您想将 startID 视图约束到另一个视图，您应该传入该视图的 resourceId。**如果要约束到父节点，那么传入 0。**
4.  endSide——作为 endID 传递的视图的一侧，您希望将 startID 视图**约束到**。

**DisconnectConstraint** 只有两个参数描述了视图和该视图要清除的约束侧。

1.  startID 要从中删除约束的视图的 resourceId。
2.  startSide 作为 startID 传递的视图中要删除约束的一侧。这遵循与前面相同的模式，只对应于 ConstraintSet 类中的常量。

为了更好的理解，我们来看一个例子。

在 XML 中，为了进行约束，我们描述了要附加的边和视图/父视图。在 ImageView 中我们应用***app:layout _ constraint top _ toBottomOf = " @ id/title "***，和说的一样；

这应该读起来非常相似；XML 中唯一没有提到的是 startID 的视图引用，因为这是我们应用属性的视图。

# 将所有内容整合在一起的扩展

这里我们有一个扩展函数，它接受一列 ContraintInstuctions 并将它们应用于 ConstraintLayout 的子视图。我们首先创建一个**constraint set**对象和 ***clone()*** 布局的当前约束。然后我们遍历指令并匹配类类型。每当我们想要连接约束时，我们使用 ConstraintSet 函数***connect()；*** 它接受 ConnectConstraint 中的相同参数。相反，如果我们想从视图中移除约束，我们使用 ConstraintSet 函数 ***clear()*** 并在 DisconnectConstraint 中传递相同的参数。最后，我们将新的约束集更改应用到布局和 BOOM！我们可以通过编程快速、灵活地连接和断开约束。