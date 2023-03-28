# 了解 Swift UI 中的 HStack、VStack 和组。

> 原文：<https://medium.com/geekculture/understanding-hstack-vstack-and-groups-in-swift-ui-e2f2e138f0e6?source=collection_archive---------8----------------------->

![](img/8a64ae912a7080aaf05dca663d349c38.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天，我们将深入了解 Swift UI 的一些基础知识。许多 iOS 开发人员现在已经听说了 Swift UI，并且可能想要开始这条新开发道路的旅程。刚开始时，它可能会令人生畏，因为 UI 元素的构造与 UI 工具包的构造有很大不同。然而，一旦您开始理解 Swift UI 的基础，您将会看到为您的应用程序快速、轻松地构建漂亮的 UI 设计是多么容易。

**HStack:**

我们今天要讨论的第一个对象是 Swift UI 中的 HStack。你可能已经猜到了，HStack 代表“水平堆栈”。这是您将在视图中调用的方法，用于创建水平堆叠的项目。这些项目可以是文本项目、图像、按钮等。您可以使用以下语法在视图中创建 HStack:

```
struct MyView: View {
   var body: some View {
      HStack {
         Text("Hello")
         Text("World!")
      }
   }
}
```

简单地创建堆栈，然后在其中创建对象。这将使您在视图中创建的对象以水平方式对齐。

**VStack:**

我们今天要讨论的下一个对象是 VStack。VStack 代表“垂直堆栈”。可以想象，这个堆栈会将对象垂直放置在其中。对象将根据您在 VStack 中列出的顺序从上到下放置。您可以像创建 HStack 一样创建 VStack:

```
struct MyView: View {
   var body: some View {
      VStack {
         Text("Hello")
         Text("World!")
      }
   }
}
```

**套料方式:**

在创建视图时，您经常会嵌套不同的堆栈(VStack、HStack 和 ZStack(将在另一个讨论中介绍))来获得您想要创建的 UI。嵌套这些堆栈是一个非常简单的过程，只需要在现有的堆栈中创建所需的堆栈。你可以在下面看到一个例子:

```
struct MyView: View {
   var body: some View {
      VStack {
         Text("Hello")
         Text("World")
         HStack {
            Text("This")
            Text("Is")
            Text("Horizontal")
         }
      }
   }
}
```

**组别:**

在 Swift UI 中创建最佳 UI 时，您可能会遇到编译器错误。当您在 UI stacik(如 VStack)中添加了 10 个以上的对象时，会出现这种情况。这是使用分组方法发挥作用的时候之一。您可以在设计中使用分组方法来解决 10 个对象的限制。每个组中也只允许有 10 个对象，但这将允许您相当无缝地设计您的 UI，而不会遇到编译器错误。下面列出了一个例子:

```
struct MyView: View {
   var body: some View {
      VStack {
         Text("Hello")
         Text("World")
         HStack {
            Text("This")
            Text("Is")
            Text("Horizontal")
            Group {
               Text("This")
               Text("Is")
               Text("A")
               Text("Group")
               Text("Within")
               Text("A")
               Text("Horizontal")
               Text("Stack")
            }
         }
      }
   }
}
```

该组将符合嵌套在其中的任何视图堆栈。这个例子符合 HStack 视图，每个文本对象将被水平显示。当创建复杂的 UI 设计和处理 Swift UI 上的当前限制时，适当地利用组将成为一种必要。

**结论:**

如您所见，在 Swift UI 中利用堆栈是一个非常简单的过程，它将允许您轻松地创建复杂的 UI 设计。这些堆栈是等同于 UI 工具包 UIStackView 的 Swift UI。您可以随意创建一个新项目，使用我们今天讨论过的不同堆栈，看看它们的运行情况，并完全理解它们的功能和布局。