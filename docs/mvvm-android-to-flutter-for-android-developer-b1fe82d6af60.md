# MVVM Android 即将上市|面向 Android 开发者

> 原文：<https://medium.com/geekculture/mvvm-android-to-flutter-for-android-developer-b1fe82d6af60?source=collection_archive---------22----------------------->

大家好，

我本人是一名 android 开发人员，我们知道大多数开发人员通常在具有**架构**的 android 平台上工作，以获得更好的代码维护和可靠性。最常用的建筑是 MVVM，MVI 或任何我们自己做的:p

所以在这里我们将会看到更多关于 MVVM 在 flutter 中的情况，这对想要在 Flutter 中编写代码的 android 开发者很有帮助。

让我想想，

![](img/6e845bf6a04ed479c72e3157bda05657.png)

Photo by [Edi Libedinsky](https://unsplash.com/@supernov?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在开始之前，那些有问题的人“MVVM 到底是什么？”让我们来看看答案。

> 模型-视图-视图模型**模式**
> 
> 在 **MVVM 模式**中的主要参与者是:**视图**——通知视图模型用户的动作。 [**视图模型**](/upday-devs/android-architecture-patterns-part-3-model-view-viewmodel-e7eeee76b73b) —展示与视图相关的数据流。数据**模型** —抽象数据源。ViewModel 使用 DataModel 来获取和保存数据。

[](/upday-devs/android-architecture-patterns-part-3-model-view-viewmodel-e7eeee76b73b) [## Android 架构模式第 3 部分:模型-视图-视图模型

### 在开发 upday 应用程序的前六个月中，经过四种不同的设计，我们了解到一个重要的…

medium.com](/upday-devs/android-architecture-patterns-part-3-model-view-viewmodel-e7eeee76b73b) 

简单来说，这只是一种遵循坚实原则的编码方式，(一种标准)。

视图—负责处理用户界面事件

ViewModel —ViewModel —负责处理 UI 事件，并在处理完成时向视图发送回调。

数据模型—视图模型使用数据模型从网络/存储中获取数据，并将其发送到**视图模型**。

理论部分到此结束，让我们把 ***挖*** 成 **F** lutter

![](img/3a785bc6a1fdfe0413b767d3bd76bcaa.png)

Photo by [Meghan Holmes](https://unsplash.com/@yellowteapot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

lutter 是 Google 的 UI 工具包，用于从单一代码库为移动、网络和桌面构建漂亮的本地编译应用程序。

是的，很优雅。

# 视角

# 视图模型

# 模型—存储库

编码部分在这里完成。不要担心实现，让我们看看用法。

通过上面的例子，你可以看到我们使用，

# **1。变更通知程序**

> **ChangeNotifier** 是包含在 **Flutter** SDK 中的一个简单类，它向其侦听器提供更改通知。换句话说，如果某个东西是一个 **ChangeNotifier** ，您可以订阅它的更改。

# **2。供应商**

> 围绕[的一个包装器继承了 Widget](https://api.flutter.dev/flutter/widgets/InheritedWidget-class.html) ，使它们更容易使用和重用。
> 
> 1.简化资源的分配/处置
> 
> 2.惰性装载
> 
> 3.大大减少了每次创建新类的样板文件

从 android 开发者的角度来看，

T**变更通知器**更像 **LiveData，**但它订阅视图中的数据(即 welcomeText 等)，当 **notifyListener()** 调用时，如果它有变更，它将触发所有数据事件，并提示 UI 进行变更。

和

T 提供者只是一种使用继承的小部件的方式，也就是说，只要简单地调用“**提供者”，我们就可以在任何需要的时候使用对象** 的 ***实例。***【法之()】。所以更像是**迪**的微缩版。

这里，我们使用一个名为 provider 的外部库。

[](https://pub.dev/packages/provider) [## 提供商| Flutter 包

### English | Português | 简体中文 | Español A wrapper around InheritedWidget to make them easier to use and more reusable. By…

公共开发](https://pub.dev/packages/provider) 

的确很美…

现在看上面的例子，

1.  [**视图**](https://pub.dev/packages/provider) 从提供者处获取 ViewModel 的实例
2.  视图使用**视图模型**中的变量绑定数据。
3.  当 provider 创建一个实例时，ViewModel 调用 Repository( **Model** )来获取数据。
4.  最后，ViewModel *触发器*将值赋给变量(welcomeText)并触发 *ChangeNotifier* 的 notifyListener() help。

从 android 开发者那里了解 Flutter Mvvm。；)

任何问题或改进或建议，请在下面随意评论。

这是一个自由的世界