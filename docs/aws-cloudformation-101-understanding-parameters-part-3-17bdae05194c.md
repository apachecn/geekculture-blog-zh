# AWS 云形成 101 —了解参数(第 3 部分)

> 原文：<https://medium.com/geekculture/aws-cloudformation-101-understanding-parameters-part-3-17bdae05194c?source=collection_archive---------28----------------------->

![](img/9f9a33b370013035aa2db99c3c42be60.png)

Photo by [Jelleke Vanooteghem](https://unsplash.com/@ilumire?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在之前的[](/geekculture/aws-cloudformation-101-how-to-use-ref-to-assemble-relationship-between-each-aws-resource-part2-9142a9d626e3)**帖子中，我分享了我如何在 CloudFormation 中学习使用`!Ref`来形成 AWS 资源之间的关系，比如给 EC2 实例分配一个安全组。**

**因此，在这篇文章中，我将分享云形成中的参数。**

****事不宜迟，我们开始吧**。**

# **云形成的参数是什么？**

**Parameters 是我们执行和运行 CloudFormation 模板时外部提供的一组输入。**

## **麦克唐纳类比**

**一个很好的类比是，你点了一份麦当劳巨无霸套餐。您可以选择的饮料**

*   **可口可乐**
*   **热咖啡**
*   **冰拿铁**

**所以这里的饮料是一个参数，因为当你点一份巨无霸套餐时，它会有所不同。**

# **为什么我们需要云形成的参数？**

**在之前关于 CloudFormation 的帖子中，我们基本上定义/硬编码了 CloudFormation 模板中的所有**属性**。那么**为什么**我们需要参数呢？我很高兴你问了这个问题。**

**原因是我们希望拥有**能力**和**灵活性**来改变属性值。尤其是那些在不同环境和参数下会发生变化的属性，使我们能够做到这一点。**

## **麦克唐纳类比**

**还记得我们之前用的麦当劳类比吗？我可以多走一步，比如**

*   **周一点一套**巨无霸配可口可乐**。**
*   **周二点一份**巨无霸配拿铁**。**

**所以，上面的例子实际上意味着我将在不同的一天喝不同的饮料。这里的“天”你可以把它和环境联系起来。**

**例如，我将在不同的环境中为我的 EC2 实例选择不同的**实例类型**。**

# **我们如何在 CloudFormation 模板中定义和使用参数？**

**至此，我们了解了什么是参数以及它们在 CloudFormation 模板中扮演的角色。**

**让我们继续讨论如何在 CloudFormation 模板中定义和使用参数。**

**接下来的例子是一个**实际的**例子，其中我们将有一个名为 **EC2InstanceType** 的参数，当我们提供 EC2 实例资源时，我们将使用这个参数作为我们的 InstanceType 输入。**

**在上面的代码片段中，我在**实例类型**属性中使用了代码为`!Ref EC2InstanceType`的参数。如果不清楚`!Ref`，可以参考我之前的[贴](/geekculture/aws-cloudformation-101-how-to-use-ref-to-assemble-relationship-between-each-aws-resource-part2-9142a9d626e3)。**

**这是一个**实际用例**，因为我们可能希望在开发环境中拥有**自由层实例(t2.micro)** ，在暂存或生产环境中拥有更高的处理能力实例。**

# **结论**

**在这篇文章中，我分享了**

*   ****使用 McDonald 类比，AWS CloudFormation 中的参数是什么**？**
*   ****为什么我们需要参数**，参数的工作是什么？**
*   **最后但同样重要的是，如何使用 CloudFormation 模板中的`!Ref`来定义和使用参数。**

**这篇文章的目的是让 AWS 初学者了解 AWS CloudFormation 中的参数。如果有任何对你来说仍然复杂的部分，感谢反馈。请在回复中告知我，以便我改进和简化它**

**感谢阅读。**