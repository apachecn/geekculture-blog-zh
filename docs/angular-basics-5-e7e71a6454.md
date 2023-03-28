# 角度基础-5

> 原文：<https://medium.com/geekculture/angular-basics-5-e7e71a6454?source=collection_archive---------16----------------------->

![](img/d5fb0cd45fc43cb9016ea6f3586495d2.png)

Photo by [XPS](https://unsplash.com/@xps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/desktop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

欢迎来到角度基础的第五部分。在这一部分中，我们将深入研究服务和依赖注入。

Angular 中的服务是一个中央存储库，可以从任何组件访问。项目的起点可以取自[这个](https://drive.google.com/file/d/1kEoiW-Zplf53TLz5G-sMhvcbqhMooWpl/view?usp=sharing) zip 文件。

我们的小应用程序有三个小组件，我们传递日志和其他东西的数据。

![](img/9a2eb7f85aee5adaca04a178a260f6bb.png)

Basic App

我们将首先创建一个日志服务。所以，在 **app** 文件夹里面创建一个文件 **logging.service.ts** 。我们将把以下内容放在里面。

![](img/6efb79d07db0892e1c305b471fd8b11f.png)

logging.service.ts

现在，我们将在我们的 **new-account.component.ts** 文件中使用这个日志服务。这里，我们必须在提供者中使用它，然后在带有类型定义的构造函数中使用它。

![](img/820e6678e312a7bc347066571238d72d.png)

new-account.component.ts

现在，我们还将在 **account.component.ts** 文件中添加日志服务。

![](img/5fbdfc2283265cac24ec3d7b62ca7b0c.png)

account.component.ts

现在，当你检查应用程序时，它们都工作正常。

![](img/08b1489a3e559bca6e2e087ddea2a723.png)

localhost

现在，我们将研究服务的另一个用户案例，即存储和管理数据。我们将创建一个服务 **accounts.service.ts** ，它将包含我们的帐户详细信息，该信息当前位于 app.component.ts 文件中。

它将包含 accounts 数组以及一个 **addAccount** 和 **updateStatus** 函数。

![](img/709e67b089deee30af801bc26cbd3da9.png)

accounts.service.ts

之后，从**app.component.html**文件中删除 accountAdded 和 statusChanged 函数。

![](img/37f9e402da77b804d88571ec0bc5214c.png)

app.component.html

现在，在 **app.component.ts** 文件中，我们将删除数组和函数，并调用我们的帐户服务。

![](img/3d3ffc8aaffba03882e0dffe2c4ee2fc.png)

app.component.ts

现在，在**new-account . component . ts**文件中，我们可以去掉发出的名称和状态。

取而代之的是，我们在其中增加了账户服务。

![](img/cd8ef19d3ba0c13d27777167fd2ae058.png)

new-account.component.ts

现在，我们将在 **account.component.ts** 文件中进行相同的更改，我们将在其中添加帐户服务并删除旧的发出的服务。

![](img/79f727688b05186d5e58441e3e41e2c2.png)

account.component.ts

现在，我们不会得到任何错误，但我们的代码被破坏，在本地主机中，我们无法添加任何服务器。

我们实际上有许多相同服务的不同实例。因此，我们将只保留 **app.component.ts** 文件的父组件中的一个。

我们将首先从 **account.component.ts** 文件中删除它。

![](img/cf3bac66fd75bd4c3e7fbcfbd3448a20.png)

account.component.ts

我们还会将它从**new-account . component . ts**文件中删除。

![](img/a70cf662cbf51ae7b2194dd66f86295e.png)

new-account.component.ts

现在，我们又可以添加新的服务器了。

![](img/6d6caecbc1a29d6405b3108e44ea84e4.png)

New server

事实上，我们还应该从 **app.component.ts** 文件中删除该服务。

![](img/b13d4745f97ae1f67b750202f96dd3c7.png)

app.component.ts

我们将把它添加到尽可能高的位置，即 **app.module.ts** 文件。

我们还将学习将一个服务注入到另一个服务中，因此我们也将在 **app.module.ts** 文件中添加日志服务

![](img/de17ea85ed6a4adc2d3634f6c5aa7116.png)

app.module.ts

接下来，我们将从 **account.component.ts** 文件中删除日志服务。

![](img/fabe6053fcb1fc49bd2fbd5078a23621.png)

account.component.ts

我们还将从 **new-account.component.ts** 文件中删除日志服务。

![](img/355259db74f4efc69570126347c27bcd.png)

new-account.component.ts

现在，我们将在 **accounts.service.ts** 文件中添加日志服务。我们还必须使它具有可注入性，因为我们要向另一个服务添加一个服务。

![](img/87a2d2191ea68a1997a3f10871a992aa.png)

accounts.service.ts

现在，我们将学习使用服务进行跨组件通信。在帐户服务中，我们希望提供一个事件，我们可以从一个组件发出该事件，并在其他组件中捕获该事件。

因此，在 **accounts.service.ts** 文件中，我们将有一个 EventEmitter。

![](img/a0f9b77f84d0c3c600f0652115123ddf.png)

accounts.service.ts

现在，我们将首先从 **account.component.ts** 文件中发出事件。

![](img/8c240aeed712f9d4b8f61ddc9fcd25d3.png)

account.component.ts

现在，我们将通过订阅来捕获**new-account . component . ts**文件中的事件。

![](img/197696b516e6747aa2e355cbeef38027.png)

new-account.component.ts

现在，我们能够在组件之间进行通信，警报显示在本地主机上。

![](img/8c9612e5e3aeb64e0d9f739c155460d1.png)

localhost

这就完成了我们的帖子，你可以在[github repo 中找到最终代码。](https://github.com/nabendu82/services-deepdive)