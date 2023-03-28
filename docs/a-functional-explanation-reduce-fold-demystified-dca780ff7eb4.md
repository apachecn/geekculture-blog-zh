# 功能解释:缩小与折叠的另一种观点

> 原文：<https://medium.com/geekculture/a-functional-explanation-reduce-fold-demystified-dca780ff7eb4?source=collection_archive---------18----------------------->

想象你正试图向你的朋友描述摩天大楼是如何建造的。(同时想象一下，你对建造摩天大楼的实际过程只有非常有限的了解。)你告诉他:

> 开始时，你需要打下基础。在那之后，你只是不断地在彼此的顶部添加楼层。简单！
> 
> 当然，实际的建造过程将根据给定楼层的用途逐层改变。办公楼层和带餐厅的楼层是不同的。这就是为什么当工人们正要开始再建一层楼的时候，他们会去找你——施工经理，问你这是一个餐厅，还是一个办公室，还是一个健身房——然后他们会照此进行。请不要在办公室的墙上挂大镜子！

不知不觉中，你正好描述了`reduce`的工作方式。

在三个主要的高阶函数中——`map`、`filter`和`reduce`——`reduce`是最强大的，但也是最容易被误解的。让我们以 Javascript 和 Haskell 程序员都能理解的方式解开这个谜团。

这是函数式解释系列的第一篇文章，我的目的是从非传统的角度解释有用的函数式编程概念。我领导一个函数式编程班，许多想法都是在教学过程中有机地产生的。

# 减少，还是建设？

`reduce`背后的想法很简单。有些东西，比如我们例子中的摩天大楼，可以通过重复许多彼此相似的简单步骤来迭代地建造。一个`reduce`允许你简单地通过提供建造所需的最低限度来建造这样的东西:

1.  你从什么开始？在我们的例子中，摩天大楼的地基。
2.  构建步骤是什么样子的？对我们来说，这是“在现有的中级摩天大楼上增加一层楼”——确切的过程取决于我们应该建造哪种类型的楼层。
3.  什么是构建计划，或者有时什么是构建块。在摩天大楼的情况下，楼层及其相关类型的列表，如“餐厅”或“办公室”。

就是这样。Reduce 迭代地执行我们的构建步骤，使用它根据构建计划向中间摩天大楼添加楼层。当建筑平面图中什么都没有时，建筑过程就停止了。

![](img/9b126dbf865705f77628641599b28ffc.png)

The legendary Tower Bloxx in which you build a skyscraper by dropping blocks on top of each other. The sole source of my skyscraper-building knowledge.

如果你仔细想想，这个概念是非常普遍的——很多东西可以通过小步骤反复构建。这意味着理解 reduce 在许多现实情况下会派上用场！让我们在下一节看一些例子。

## 一旦你看到它…

对一系列数字求和过吗？嗯，难道那个*真的不就是*只是一种根据指令列表建立数字的方式吗？

*   基础= 0
*   积木=要求和的数字列表
*   构建步骤=将构建块列表中的数字(“当前楼层”)添加到我们得到的中间结果(正在建造的摩天大楼)中

重复迭代，然后砰！你自己有一笔钱。产品或字符串列表的连接也是如此。这就是`reduce`得名的原因——您将一个元素列表简化为一个汇总值。

根据一些谓词过滤元素数组，只保留那些谓词返回 true 的元素，怎么样？

*   您正在构建一个数组，从一个空数组开始
*   构建块是输入数组中的元素
*   构建步骤包括将当前元素(您将要构建的楼层)添加到中间结果(摩天大楼)中，或者什么都不做(…并继续下一层)

抛开玩具示例，web 服务器也可以被视为一个大的简化操作。你有没有一些内部状态，你会根据一系列传入的请求而改变？对我来说听起来像是降价！当然，谈论“建立内在状态”看起来很神秘，但是从某个角度来看，你确实在这么做。构建计划由传入的请求组成。以及说明当您收到某个请求时内部状态应该发生什么的函数一起描述了构建步骤。

希望 reduce 现在更有意义，作为一种通过描述一个构建步骤的样子来迭代构建东西的操作。在我们告诉你如何在实践中使用它之前，让我们来看看最后一块缺失的拼图。

## 实施

让我们探索一下如何实现`reduce`;先说类型。探戈需要两个人，而建造摩天大楼需要三个人:地基、建造计划和建造步骤。因此，一个专门用于摩天大楼建设的`reduce`会有这样的类型:

```
reduce(
   floorTypes: List of Floor Types,
   buildStep: 
     (currentFloor: Floor Type, intermediate: Skyscraper) 
      => bigger Skyscraper,
   foundation: a very very small Skyscraper, 
): Skyscraper
```

当然，我们已经看到`reduce`是更通用的，所以我们可能不应该只针对摩天大楼的类型。更一般的类型看起来更像下面这样:

```
reduce<Block, Thing>(
   buildingBlocks: List of Blocks,
   buildStep: (current: Block, intermediate: Thing) => next Thing,
   initialValue: some initial Thing, 
): Thing
```

那和它在 Typescript 里的类型很像！甚至在 Haskell 中，我们现在可以看到我们的`reduce`版本和他们的`foldr`版本之间的相似之处:

```
 build step  foundation    result
                       vvvvvvvvvvvv     v           v
foldr :: Foldable t => (a -> b -> b) -> b -> t a -> b
                                             ^^^
                                   a "list" of building blocks
```

当然，Haskellers 做了额外的努力，允许我们在任何合理的集合中提供构建块`t`——不仅仅是一个列表，而是一个数组、一个集合，或者更一般地，任何实现`Foldable`“接口”的东西。

不管怎样，现在我们有了类型，那么实现会是什么样子呢？你也许可以自己拼凑起来。在伪代码中:

```
function reduce(buildingBlocks, buildStep, initialValue):
  intermediateResult = initialValue
  for block in buildingBlocks:
    # Place one block on the current structure
    intermediateResult = buildStep(block, intermediateResult)
  # We ran out of building blocks, it's time to...
  return intermediateResult
```

少了一个需要担心的功能概念。

# 结论

正如你所学的，一个有用的方式来看待`reduce`是把它看作一个**函数，使你能够构建东西**。您提供一组构建块，描述一个构建步骤是什么样子的，以及您构建的基础是什么，其余的由`reduce`负责。特别是，它遍历您提供的所有构建块，通过使用提供的构建步骤函数将它们添加到中间构建中。

这个概念非常普遍，它包含了我们在日常编程中所做的大部分事情，从数字求和到构建 web 服务器。**这是不是意味着凡事都要用 reduce？请不要，尤其是如果你不写哈斯克尔。**大多数常见用例已经有了一个专用函数，比如`map`或`filter`或`sum`，而大多数不常见的复杂用例最好使用 for 循环和辅助函数。

在中间的情况下，你*将*开始使用 reduce，你肯定会在代码库中看到它，所以很高兴你现在对它如何工作有了直观的理解。在 Haskell 中更是如此，其中`foldr`和`foldl`是显式和手动编写递归的流行替代方法。

少了一个需要担心的功能概念——你必须承认，它现在看起来没那么复杂了。等到你学会了，其他大部分功能概念也是如此。但稍后会详细介绍。敬请关注本系列的下一篇文章*功能解释*。

`fold`只是缩减操作的不同名称。这个名字可以在 Haskell、Purescript 和其他函数式语言中看到。

实际上，如果你足够努力——不是说你应该尝试——大多数循环和递归函数都可以用`reduce`编写。