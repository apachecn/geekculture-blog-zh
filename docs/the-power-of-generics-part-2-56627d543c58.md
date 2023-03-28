# 泛型的力量第 2 部分

> 原文：<https://medium.com/geekculture/the-power-of-generics-part-2-56627d543c58?source=collection_archive---------34----------------------->

![](img/1eceea1dc69c95331c9a8a0d747b1551.png)

Photo by [Markus Spiske](https://www.pexels.com/@markusspiske?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/creative-internet-designer-abstract-6212801/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

**学习成果**

1.  了解如何以及何时使用具有关联类型的协议。
2.  了解如何以及何时使用一般的 where 子句。

在[之前的泛型文章](/geekculture/the-power-of-generics-ba37053a95d9)中，我们讨论了泛型的主要主题:类型参数、泛型函数、泛型类型和类型约束。现在是时候进入下一个阶段了🚀。在本帖中，我们将介绍:

1.  关联类型:在我们的协议中添加类型参数。
2.  泛型 Where 子句:在我们的类型参数中添加附加条件。

# 关联类型

关联类型是添加类型参数的方式，作为我们协议的一部分。我们可以在协议函数中使用这些类型参数作为属性类型或返回/参数类型。另一件重要的事情是，这些关联的类型也可以有约束，这意味着协议实现必须遵守这些类型约束中的任何一个。关联类型允许我们拥有具有通用功能和通用属性的协议，为什么？因为协议实现将**负责**为它实现的协议中的每个关联类型分配一个实际类型。

先说一个小例子。

在前面的例子中，我们创建了一个名为`MyProtocol`的协议，并添加了一个名为`MyAssociatedType`的关联类型。向协议中添加关联类型很容易，我们只需编写关键字`associatedtype`,后跟关联类型名。我们的协议中另一件重要的事情是，它有一个函数接收类型为`MyAssociatedType`的参数，并返回类型为`MyAssociatedType` 的值。我们现在知道`MyAssociatedType` 是哪种型号了吗？不，我们不需要，因为这是任何`MyProtocol` 实现都要考虑的问题。到目前为止，我们唯一知道的是所有的实现都必须有一个带有类型为`MyAssociatedType`的参数的函数，并且这个函数也必须返回类型为`MyAssociatedType`的值。实现还必须遵循我们的关联类型中的类型约束:`MyAssociatedType: Equatable.`

现在让我们看看`MyProtocol`的实现，这是事情开始变得更有趣的地方。在`MyProtocolImplementation`类中，我们告诉编译器我们的关联类型将是`EquatableEnum`类型的。怎么会？你可能会问，从我们实现`myFunction`并使用`EquatableEnum`作为参数和返回类型的那一刻起，我们就告诉编译器在这个实现中使用`EquatableEnum`作为关联类型。这是一种隐含的方式，我们正在利用 Swift 编译器类型推断。我们可以通过添加一个`typealias`以显式方式完美地做到这一点，如下所示:

现在我们来看看`MyProtocol`的第二个实现。在第一个实现中，我们希望通过接收和返回一个`EquatableEnum`来处理枚举。在`AnotherImplementation`类中，我们希望通过接收和返回一个`EquatableStruct`来处理一个结构。这同样适用于这里，从我们使用`EquatableStruct`作为`myFunction`的参数和返回类型开始，我们就告诉编译器`EquatableStruct`将是我们在这个实现中的关联类型。正如你所看到的，由于关联类型，我们有两个非常不同的`MyProtocol`实现，一个使用枚举，另一个使用结构。

如果出于某种原因，我们决定在任何一个实现中改变`myFunction`的返回类型，我们将得到一个编译器错误，告诉我们我们的实现不符合协议。为什么？因为返回和`myFunction`的参数类型必须相同，如果我们只改变参数类型，我们将会有相同的错误。如果我们也决定使用不符合`Equatable`协议的类型作为`myFunction`的参数/返回类型，我们将会得到同样的错误。

我们可以自由决定使用哪些类型作为关联类型，只要我们尊重任何现有的类型约束和函数/属性类型。

## 依赖于泛型类型的协议

到目前为止，我们已经讨论了在协议中使用关联类型的第一个场景。但是还有另一个，想象我们有一个协议，或者有一个接收/返回泛型类型的函数，或者有一个泛型类型的属性。

让我们看一个例子:

在前面的例子中，你可以看到我们声明了一个名为`MyModel`的结构。这个结构是一个泛型类型，它接收一个名为`Type`的类型参数。`Type`被约束为一个`Equatable`实现。我们还声明了`AnotherProtocol`协议，该协议有一个接收类型为`MyModel`的参数的函数。我们刚刚提到`MyModel`是一个泛型类型，这意味着我们必须向每个`MyModel`声明传递一个类型参数。要做到这一点，我们必须定义一个关联的类型，并将它传递给`parameter`声明。在协议中与泛型交互的唯一方式是通过关联类型，因为协议不允许泛型参数。您首先想到的可能是向协议传递一个类型参数，如下所示:

这段代码将抛出下一个编译器错误:

```
Protocols do not allow generic parameters; use associated types instead
```

现在您知道了在协议中使用关联类型的另一种有用的方法。总结关联类型在以下两种情况下很有用:

1.  通过拥有泛型参数和返回类型来灵活实现协议。
2.  声明依赖于泛型类型的协议。

## 将具有关联类型实现的协议用作属性类型

假设我们想要声明一个类，它有一个类型为`AnotherProtocol`的属性，这是我们在前面的例子中定义的协议，记住这个协议有一个关联的类型。

我们不能做这样的事情:

```
let myConstant: AnotherProtocol
```

编译器将抛出下一个错误:

```
Protocol 'AnotherProtocol' can only be used as a generic constraint because it has Self or associated type requirements.
```

我们的类拥有类型`AnotherProtocol`属性的唯一方法是创建一个泛型类型，该类型接收一个被约束为`AnotherProtocol`实现的类型参数。让我们看一个例子:

在前面的例子中，我们创建了`MyClass`通用类型。该泛型类型接收被约束为实现`AnotherProtocol`协议的`Type`类型参数。然后我们声明了一个类型为`Type`的私有常量，这样我们就可以访问`AnotherProtocol`实现并调用它可能有的任何方法。

现在假设我们想要创建一个新的协议，它有一个类型为`AnotherProtocol`的属性。协议不能有类型参数，所以我们不能像前面的例子那样做。为此，我们必须添加一个关联类型，并将其约束为`AnotherProtocol`的实现。

# 通用 Where 子句

除了类型约束之外，我们还有一般的 where 子句来为我们的类型参数添加额外的条件。通用子句有助于:

1.  比较泛型类或函数中的类型参数，
2.  将类型参数的关联类型与泛型类或函数中的其他类型参数进行比较，并
3.  确保关联的类型符合某些协议或等于特定类型，如`Int`或`Bool`。

假设我们想在类中添加一个泛型函数。这个通用函数接收一个类型为`T`的参数，`T`是一个类型参数，被约束为一个具有相关类型的协议的实现。我们希望我们的函数只与具有`Equatable`关联类型的协议实现一起工作。听起来有很多代码，对吗？这很容易做到。

让我们首先用泛型函数创建我们的类，用相关类型创建我们的协议:

这就是我们要做的！我们来看看`myFunction`。如您所见，它接收了一个类型为`T`的参数，这是一个被约束为`MyProtocol`的实现的类型参数。`MyProtocol`有一个关联的类型，我们提到过我们只希望我们的函数与那些有`Equatable`关联类型的`MyProtocol`的实现一起工作。要添加`Equatable`验证，我们必须使用 where 子句。

现在让我们假设我们想要定义一个接收两个类型参数`Type1`和`Type2`的泛型类。`Type1`必须被约束为`MyProtocol`实现，`Type2`没有任何类型约束。我们的泛型类将有两个属性:`Type1`类型的`myProtocolImplementation`和`Type2`类型的`anotherProperty`。我们想要创建一个函数来比较类型为`AssociatedType`的`myProtocolImplementation`存储属性`myProperty`和类型为`Type2`的`anotherProperty`。

在那里！正如您所看到的，我们用一般的 where 子句向类型参数添加了额外的条件。首先，我们有一个类型为`Type1`的属性`myProtocolImplementation`和类型为`Type2`的属性`anotherProperty`。现在我们想比较包含在`MyProtocol`实现中的`myProperty`和`anotherProperty`并返回一个布尔 but，如果我们不知道它们是哪种类型，我们怎么能比较它们呢？如果我们不知道它们是不是`Equatable`，我们怎么能比较它们呢？我们可以用一个通用的 where 子句来添加条件，以确保这一点。第一个条件帮助我们确保`Type1.AssociatedType`和`Type2`是相同的类型。另一方面，第二个确保`Type2`是`Equatable`，因为`Type1`等于`Type2`，所以我们确定它们都是`Equatable`。

在这个例子中，我们在 where 子句中有多个条件，如果我们有多个条件，我们必须用逗号分隔它们，如下所示:`where Type1.AssociatedType == Type2, Type2: Equatable`

## 扩展和通用 where 子句

有时我们可能想给特定的类型添加一个扩展，但是只在特定的条件下。为此，我们也可以使用一般的 where 子句。

假设我们想给一个数组添加一个扩展，但是只给`[Int]`数组添加，这个扩展将有一个名为`sum`的函数来返回数组中元素的总和。让我们先说 Swift 中的数组是接收一个`Element`类型参数的通用类型。现在我们有了更多的上下文，让我们创建我们的扩展。

在前面的例子中，正如你所看到的，我们只对那些`Element`等于`Int`的数组进行了扩展。