# 360 IT Check #19 — Next.js 12、Android Dev Summit、GitHub Universe 等等！

> 原文：<https://medium.com/geekculture/360-it-check-19-next-js-12-android-dev-summit-github-universe-and-more-cd2199a66f85?source=collection_archive---------11----------------------->

![](img/6c90dbd0a75c548b71e4b7c8bf66b80c.png)

*02/11/2021 16:34 更新了文章，增加了* [*链接到我们的 GitHub 资源库*](https://github.com/itmaginationdemos/Jetpack-Compose) *，在这里我们演示了最新的 JetPack Compose，以及 Compose Material 3。*

*02/11/2021 17:27 更新了文章以包括* [*一个链接到我们的文章详细介绍 Next.js 12*](https://www.itmagination.com/blog/next-js-12-release) *&它带来的变化。*

# Android 开发峰会

上周，谷歌为 Android 开发者组织了一次峰会，在此期间，我们看到了大量的技术讲座。除了如何制作更好的原生应用程序的技巧，这家来自山景城的公司还发布了几项声明。

[12L 是大屏幕设备的新操作系统](https://developer.android.com/about/versions/12/12L)。因此，它被构建为充分利用可用的屏幕空间——每平方厘米都在使用中；这是平板电脑操作系统过去没有做到的。喜欢一心多用的人应该有宾至如归的感觉。有了新的任务栏和易于调用的分屏模式，拥有一个大屏幕开始有所回报。所有开发者都有时间让他们的应用适应新的操作系统。[谷歌分享了一些指导和样本代码，让这个过程更加顺畅。有趣的是，这家互联网巨头也将发布改进，通过新的](https://developer.android.com/about/versions/12/12L#optimize-for-large-screens) [WindowManager API](http://d.android.com/guide/topics/large-screens/learn-about-foldables) ，使可折叠屏幕的开发更加顺畅。

下一个宣布的新颖性是**作曲素材 3** 。

Compose Material 3 是 Jetpack Compose 的库:“Android 构建原生 UI 的现代工具包。”它是新的酷小子，帮助开发人员“用更少的代码、强大的工具和直观的 Kotlin APIs”将他们的应用程序变成现实。

更新的 Compose Material 实现了 Material You，这是 Google 设计系统的最新版本。它具有大量的新功能，例如，但不限于，动态颜色和小部件更新。

我们已经为您准备了一个资料库，让您可以立即试用最新的功能。这里的见代码[。](https://github.com/itmaginationdemos/Jetpack-Compose)

# Next.js 12

Next.js 12 是世界上最流行的 React 混合 web 框架的最新版本。因此，我们准备了一个最新的主要更新的演练。你明天就能读到它了。敬请期待！

**更新**:检查 Next.js 12 变化的文章现在在我们的博客上。[点击这里阅读。](https://www.itmagination.com/blog/next-js-12-release)

# GitHub 宇宙

GitHub Universe 也在上周为 GitHub 用户举办了为期两天的活动。

我们选择了一些我们认为您应该密切关注的公告:

# 新的命令选项板

VS 代码风格的调色板来到了 GitHub。它的目的是让你随时掌握所有有用的命令。用法非常简单。在 Windows 和 Linux 上按下 *Ctrl + K* 或 *Ctrl + alt + K* 打开命令面板，在 macOS 上按下 *Command + K* 或 *Command + Option + K* 打开命令面板。还有其他模式让你的生活更轻松。

[来自 GitHub 的博客](https://github.blog/changelog/2021-10-27-command-palette-beta/):

*   >进入命令模式
*   #搜索问题、拉动式请求、讨论和项目
*   ！搜索项目
*   @搜索用户、组织和存储库
*   /在存储库范围内搜索文件

# 改进的 CI/CD

GitHub 最近新增的产品是“GitHub Actions”该功能可能会引入到竞争对手 GitLab，这是一个流行的 DevOps 平台。可以使用 OpenID Connect 保护部署，还有自动扩展功能等等。

详情请看 GitHub 的[这段视频](https://www.githubuniverse.com/2021/session/689506/github-actions-in-action)。不过，录像在一面登记墙后面。

# GitHub 代码空间的改进

Codespaces 是一种在云中编码的方式，就像在计算机上编码一样。GitHub 声称，多亏了它，他们能够将构建新开发环境的时间从 45 分钟缩短到 10 秒钟[原文如此！].上周他们推出了新功能。

其中一些是:

*   **更简单的开发环境创建:**您现在可以通过一次点击设置，创建和更新作为代码定义的 *devcontainer.json* 开发环境。
*   CLI 支持:我们在 GitHub CLI 中添加了代码空间支持，以帮助喜欢命令行和直接 SSH 访问开发环境的开发人员。
*   测试版中的 REST API 支持:一个新的 REST API 使得以编程方式管理代码空间变得更加容易，包括机器类型和秘密。
*   **转发端口的访问控制:**将转发端口共享到您的代码空间，并将它们标记为公共、私有或与您组织的成员共享。
*   **无缝访问 GitHub 容器注册中心:**自动认证存储在 GHCR 中的 dev 容器，无需提供个人访问令牌(PAT)。

# GitHub Copilot 支持 JetBrains IDEs 和 Neovim

GitHub Copilot， [dev 社区最喜欢的 AI 自动完成引擎](https://www.itmagination.com/blog/rise-ai-code-autocompletion-engines-github-copilot-tabnine-kite)，正在扩展对 JetBrains 的一系列 ide 的支持，如 IntelliJ IDEA、Goland 或 Rider 和 Neovim。现在可以享受智能代码完成的开发人员数量显著增加，而且公司可能会决定进一步扩展其兼容性。

*原载于*[*https://www.itmagination.com*](https://www.itmagination.com/blog/360deg-it-check-19-next-js-android-dev-summit-github-universe)*。*