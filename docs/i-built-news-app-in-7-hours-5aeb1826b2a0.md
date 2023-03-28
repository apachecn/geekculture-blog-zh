# 我在 7 个小时内创建了新闻应用程序

> 原文：<https://medium.com/geekculture/i-built-news-app-in-7-hours-5aeb1826b2a0?source=collection_archive---------10----------------------->

![](img/8783f78b3b347354a17df23c2eacc065.png)

Photo by [Alvaro Reyes](https://unsplash.com/@alvarordesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我有了为科技、创业和金融领域的新闻和文章创建一个应用程序的想法。所以我决定造一个。在本文中，我将向您展示我是如何快速构建我的应用程序的。

# 使用应用程序用户界面

我遵循的第一件事是用硬编码的数据创建应用程序，这样我就能感受到应用程序在获得数据后的样子。为了创建用户界面，我通常浏览 dribble 来获得用户界面的想法。现在我知道我希望我的应用程序是什么样子了，是时候开始编码了。

# 开始编写应用程序 UI

您可以为您的应用程序创建用户界面，并从硬编码您想要显示的数据开始。稍后，您可以从在线服务器获取这些数据。

# 寻找完美的 API 来获取内容

在这个应用程序的想法中，我不想创建所有的后端基础设施，以便我可以继续在这个利基拉内容，我只想专注于建立我的应用程序，而不是后端。所以我开始寻找一个 API 来获取这个领域的内容。

然后我决定使用**QuickFloat.com**API，我只需要调用一个 GET API 就可以获得该类别的最新内容。

# 与 API 集成

现在是将 UI 与 API 集成的时候了，这样应用程序就可以显示来自 API 的内容。对于 API 调用，我使用了 retrofit，这是一个轻量级的 HTTP API，它可以减少调用 API 和获取数据所需的代码。

# 准备播放商店列表

*   **图标**:为了给我的应用程序生成图标，我通常用 https://romannurik.github.io/AndroidAssetStudio[的](https://romannurik.github.io/AndroidAssetStudio)
*   **应用截图**:为了免费创建优秀的应用截图，我去了[https://appscreens.com/](https://appscreens.com/)这是一个很棒的平台，可以让你非常快速地创建应用截图。
*   **特色图片**:您需要将特色图片上传到应用的 play store。我用 canva.com 做这个。因为它是免费的，而且很棒。
*   **Youtube 视频(可选)**:如果你也想创作一个好的 app 视频，你可以在 Fiverr 上找到一个视频编辑，他可以廉价地创作出好的视频。

现在你只需要完成商店列表并点击发布。

希望这篇文章能帮助你规划你的应用程序开发。

干杯！！