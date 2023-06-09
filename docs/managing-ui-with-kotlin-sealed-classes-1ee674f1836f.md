# 用 Kotlin 密封类管理 UI

> 原文：<https://medium.com/geekculture/managing-ui-with-kotlin-sealed-classes-1ee674f1836f?source=collection_archive---------5----------------------->

![](img/91b65a0d583f3e4e2687209e48af5689.png)

Photo by [Marc Reichelt](https://unsplash.com/@mreichelt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/kotlin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大家好！我叫格里戈里，来自莫斯科的 android 开发人员。这是我第一篇关于 android 应用和 kotlin 密封类的文章。这是一个简单的例子。我希望有人会觉得有用，并会很高兴收到您的反馈和想法！

在本文中，我将创建一个非常简单的 android 应用程序，向您展示用 kotlin 密封类管理 UI 的简单方法。我试图遵循基本清晰的架构和坚实的原则，但有些事情，如缺乏依赖注入，样板代码和其他小的限制，你会在这里看到在简化应用程序的情况。这篇短文的主要目的是展示我们如何使用 kotlin 密封类并以简单美观的方式管理我们的 UI。

首先创建项目，打开 app **build.gradle** 文件并添加这个。

我使用协程和 MVVM，但你可以选择 RxJava，MVP 等。现在我们需要在主活动 xml 中创建一些视图。

这个 xml 文件是为了展示主要思想而创建的，它远非完美。所以我们有三个主要视图:**成功 _ 布局**、**进度条**和**错误 _ 布局**。默认情况下，他们的可见性是**消失**。

下一步是创建 MainScreenState 类。

一个密封类的所有子类在编译时都是已知的，所以 **when** 表达式帮助我们写出清晰易懂的代码。

现在我们需要为测试创建数据类。

所有的业务逻辑都在一个视图模型中。这是一个生命周期感知类，帮助我们存储和管理在屏幕旋转等配置更改后仍然存在的数据。

在 MainViewModel 类中，我们有一个存储库。在大型应用程序中，我们通常需要一个中间层，如 interactor 或用例，来分离逻辑和责任，但是一个存储库对我们来说已经足够了。一个好的解决方案是通过 dagger、koin 或其他依赖注入框架在视图模型中注入存储库。

下一个属性是 defaultError 字符串。不要在真实的 app 里做。一个好主意是注入一个定制的资源管理器类，它有一个上下文，可以在视图模型中正确地获取资源。

我们的数据持有者是一个可变的动态数据。我们可以很容易地从活动或片段中观察到它，并向用户显示实际数据。

让我们来看看主要的 **fetchData** 方法。首先，我检查屏幕状态值。如果它已经**成功**在我们的情况下我们不需要继续加载。这真的是一个非常简单的例子。否则，我将通过加载状态来改变屏幕状态，以显示用户挂起并尝试从存储库中获取数据。当我们向服务器或数据库请求数据时，我们几乎总是需要进行阻塞操作。这里我使用协程和 viewModelScope 扩展。在这段代码中，我可以调用挂起函数——协程的主要思想。**延迟**有助于我们测试 UI，但不要在真实的实践中这么做。所有的合成负载和期望都是一个糟糕的决定。作为开发人员，我们必须尽可能快地向用户展示信息和数据。最好的解决方案是存储和显示来自缓存的数据，同时从服务器下载。下一步很明显——如果我们得到了结果，我们显示**成功**，如果没有，我们显示**错误**。

看看仓库。

我们应该使用接口并在实现中隐藏它们的逻辑，以创建维护良好的应用程序。MainApi 是一个对象——匿名类，用单一方法实现接口。它可以是改型、数据库或其他来源。

MainRepositoryImpl 的 **getData** 方法随机从 api 返回用户或者抛出异常。

通常我使用视图绑定特性来与视图交互，但是这里有一个旧的 **findViewById** 解决方案。首先，我们向 ViewModelProvider 请求我们的主视图模型。如果它存在，我们得到一个保存了所有数据的模型，如果不存在，我们得到一个新的模型。在这之后，我开始观察屏幕状态的实时数据。因为屏幕状态实时数据的类型是我们的密封类，所以当没有 else 分支的表达式时，我们可以使用**。我们绝对肯定只能得到这三个子类中的一个。在这里，我隐藏或显示主要视图，如成功布局，进度条和错误布局。**

## 结论

Android 开发中的密封类帮助我们管理 UI，编写清晰易懂的代码。但是当我们谈论一个有许多屏幕的大型项目时，这个应用程序的设计有一个很大的缺陷。这个大纰漏的名字是违反干原则的。每一个新的屏幕都迫使我们编写重复的代码。如果您喜欢这篇文章，我将创建一篇新文章，向您展示如何优雅地避免代码重复。感谢阅读！以下是[该项目的完整代码](https://github.com/Sosorevgm/android-ui-sealed-classes):[https://github.com/Sosorevgm/android-ui-sealed-classes](https://github.com/Sosorevgm/android-ui-sealed-classes)