# App 图标消失了！

> 原文：<https://medium.com/geekculture/app-icon-disappeared-e4684500fc59?source=collection_archive---------46----------------------->

![](img/e032ed11f8614457c2e253e0ea7d55ae.png)

Photo by [Katie Moum](https://unsplash.com/@katiemoum?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果我们想在移动设备中使用某个应用程序，我们只需点击想要使用的应用程序的图标，系统就会为我们打开该应用程序。如果应用程序图标在启动器或应用程序抽屉中不可用，你将如何打开应用程序，或者甚至有可能使应用程序图标从启动器中消失。

让我们找出答案。

# 应用程序启动器图标消失了！

在一个功能开发期间，我正在试验 AndroidManifest.xml 文件中的代码，并尝试应用程序的不同启动行为，然后突然应用程序图标不见了，我在应用程序启动器和应用程序抽屉中仔细寻找，但应用程序图标无处可寻。

我试图从 apk 重新安装应用程序，但它说应用程序已经安装。当我试图使用 Android Studio 运行该应用程序时，该应用程序已启动，但应用程序图标仍未出现在启动器中。

这是一种非常奇怪的行为，我以前从未经历过，甚至没有想到过，为什么这甚至是可能的，或者有没有这种行为的用例，就好像我们看不到应用程序图标一样，为什么我们甚至希望在我们的设备中有这样的应用程序。

在设置的应用程序管理器中，我可以找到应用程序，因为在设置中有所有的应用程序，但为什么应用程序显示这样的行为。太棒了。

# 调查的结果

在 AndroidManifest.xml 文件中，我们提到了关于我们的应用程序的所有重要属性，如活动、广播、启动屏幕、权限等。如果我们希望我们的应用程序图标从启动器中消失，我们可以在那里定义这种行为。

`<intent-filter>`看起来像这样，它包含当点击应用程序图标时要启动的活动

只需将`<data>`添加到意图过滤器中，例如

编译并运行它。答对了。。您的应用程序将像以前一样运行和工作，但应用程序图标将不会出现在启动器中。