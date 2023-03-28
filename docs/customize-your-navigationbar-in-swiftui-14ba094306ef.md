# 在 SwiftUI 中自定义导航栏

> 原文：<https://medium.com/geekculture/customize-your-navigationbar-in-swiftui-14ba094306ef?source=collection_archive---------3----------------------->

![](img/8a576072a61db3f2694860272b1d6c65.png)

Story Image

大家好，今天我们将谈论导航栏视图定制。事不宜迟，我们开始吧。

# 我们开始吧

上面说的定制和 UIKit 中设置`navigationView.titleview`是一回事。要在 SwiftUI 中定制导航栏标题视图，我们只需将放置类型`[.principal](https://developer.apple.com/documentation/swiftui/toolbaritemplacement/principal)`的`[ToolbarItem](https://developer.apple.com/documentation/swiftui/toolbaritem)`设置为新的`.toolbar`修饰符。这些`.principal`放置设置简单来说，将渲染视图对齐到 NavigationView 的中心。让我们看看下面的例子。

![](img/818827cf6bd6d5a25bdb8db5adfb7654.png)

Example1

在上面给出的例子中，我们创建了 NavigationView 并添加了`.toolbar`修饰符。设置位置后，您可以在**工具栏中放置任何您想要的东西。**

![](img/beca0b3e5148d99f6bcece24c70e3621.png)

Example2

这不仅限于文本，你可以把图像，按钮，无论你喜欢什么。

![](img/606b639c915369c6be4b666ba8c78f74.png)

Example1

![](img/dcf3d93467283893985450825e39c30f.png)

Example2

今天我想谈谈小 SwiftUI 的特性。在新的文章中再见。注意安全，✌🏼