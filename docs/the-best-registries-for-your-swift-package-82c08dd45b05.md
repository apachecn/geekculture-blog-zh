# 适用于您的 Swift 产品包的最佳注册中心

> 原文：<https://medium.com/geekculture/the-best-registries-for-your-swift-package-82c08dd45b05?source=collection_archive---------15----------------------->

![](img/218743f9dc596603c1989f0d6854a14f.png)

Let’s meet Swift Package Registry, Swift Package Index and Swiftpack.co

作为 Swift 开发人员，您希望人们能够快速发现您的 Swift 产品包。如果你很难找到“如何做”的答案，这是可以理解的。Swift 包管理器是一个管理 Swift 代码分发的漂亮工具。它与 Swift 构建系统相集成，可自动完成下载、编译和链接依赖关系的过程。但是说到底，*只是*一个包经理。

对于与 Swift 包裹相关的包裹注册，没有事实上的标准。私人和社区项目正试图填补更容易搜索和发现 Swift 包裹的空白。

这篇博文将

*   向您概括介绍三个最受欢迎的 Swift 包裹注册管理机构(截至 2021 年 12 月
*   *比较它们的功能集并*
*   *为软件包作者和用户提供使用哪个软件包的建议*

*根据我发现它们的时间，三个最受欢迎的注册表是*

1.  *Swift 包裹注册中心*
2.  *Swift 包裹索引*
3.  *Swiftpack*

# *Swift 包裹注册中心*

> **由于 GitHub Swift Package Registry 目前尚未发布，且 IBM Swift Package Catalog 已被取消，因此 Swift Package Registry 填补了当前的空白，可更轻松地搜索和发现 Swift 软件包。**

*![](img/0b4df936e125d0368cec7164d9a2846e.png)*

*Start site of Swift Package Registry*

*您可以通过[swiftpackageregistry.com](https://www.swiftpackageregistry.com)访问 Swift 软件包注册表。*

*下面是这个注册表显示的包信息的一个例子。*

*![](img/b239718077407098597b1b2fa2690299.png)*

*Alamofire package shown on Swift Package Registry*

*该项目是[开源的](https://github.com/twodayslate/swift-package-registry)，这个 GitHub 应用程序是用 Probot 构建的 Node.js 应用程序。它使用 Docker 解析所有 Swift 包。*

*![](img/217f0420b745a5af7e64fb7ab4f3de97.png)*

*Swift Package Registry is open-source*

*将软件包添加到注册表是很容易的。*

*![](img/8706c448d3e60cb5befbf4c5dc95083d.png)*

*Adding a package to Swift Package Registry*

*我个人的观点是:这个注册中心是第一个添加我的 Swift 包的注册中心。通过 GitHub 应用程序集成 GitHub 给我留下了深刻的印象。该项目有我最深的尊重✊，但功能是缺乏相比，其他注册表。*

# *Swift 包裹索引*

> **Swift 包索引有助于您对应用中使用的依赖项做出更好的决策。**

*![](img/89e33274159b27d49285009169bfcf27.png)*

*Start site of Swift Package Index*

*您可以通过[swiftpackageindex.com](https://swiftpackageindex.com/)访问 Swift 套餐索引。*

*下面是这个注册表显示的包信息的一个例子。*

*![](img/36eca7628b1e11b278ae638fc17e7859.png)*

*Alamofire package shown on Swift Package Index*

*该项目是[开源的](https://github.com/SwiftPackageIndex/SwiftPackageIndex-Server)，使用 Vapor 用 Swift 编写。*

*![](img/fa5e8e21e36f1783021318083c927b7f.png)*

*Swift Package Index is open-source*

*向注册表添加包需要基本的 Git / GitHub 经验。*

*![](img/3b06414d11e71995cdaa2eadec14ba09.png)*

*Instructions to add a package to the Swift Package Index*

*显著的特点是*

*   *高级搜索功能*

*![](img/5800464ee667b8de53dc1188c2e63980.png)*

*Advanced Search Capabilities through filters*

*   *自动创建包集合*

*![](img/7e4a6631b6ecd73f4a899562eaa47ca1.png)*

*Out-of-the-box Package Collection Support*

*   *能够生成已经插入了依赖项的 playground 文件，可以用 Xcode 打开。*

*![](img/00eb1e6faa8585cb916f6fa565515c4b.png)*

*Try out a Swift Package*

*   *通过 [Twitter 账户](https://twitter.com/SwiftPkgUpdates)发布包更新*

*![](img/b6b0214b8e8870b1d48915bb57cd0aa2.png)*

*A tweet for each new releases and package*

*我的个人观点:这个注册表似乎是最活跃的，因此它拥有最多的功能也就不足为奇了。🌟🌟🌟*

# *Swiftpack 包*

> **我们尽最大努力抓取 github 的新包**

*![](img/8700eb997567085aafd01afbe934bff3.png)*

*Start site of Swiftpack*

*您可以通过 swiftpack.co 的[访问 Swiftpack。](https://swiftpack.co/)*

*下面是这个注册表显示的包信息的一个例子。*

*![](img/3f266dddb0a289998c70e03d002018c3.png)*

*Alamofire package shown on Swiftpack*

*该项目是封闭代码。*

*这个注册表列出了最多的包，因为它会自动抓取 GitHub 中的新包。*

*您也可以手动注册软件包。*

*![](img/1e37bae4c6b16daa833e898aeac21aa6.png)*

*Adding a package to Swiftpack is rarely needed*

*您可以轻松地搜索/查找作者的所有包，但不支持包集合。*

*![](img/dabde6c9c30ee772d65a20512c0ee711.png)*

*Packages listed by authors on Swiftpack*

*Swiftpack 还通过一个 [Twitter 账户](https://twitter.com/swiftpackco)发布软件包更新*

*我个人的看法:这个注册表似乎是由一个人维护的。因此，特别令人难以置信的是，该注册表具有自动抓取和添加 Swift 包到注册表的独特功能👍👍👍考虑到这是一个没有外界贡献的单人项目，该注册中心是否能赶上 Swift Package Index 的功能丰富性还值得怀疑。*

# *详细比较*

*![](img/6bc3877076598f662b30bab3d7cf7c77.png)*

*Feature Set Comparison*

# *建议*

*作为软件包作者，我建议确保您的软件包被添加到所有这些注册表中。*

*作为套餐消费者，我主要使用 **Swift 套餐索引**，因为它功能丰富。*

**最初发布于*[*https://blog . ei dinger . info*](https://blog.eidinger.info/the-best-registries-for-your-swift-package)*。**