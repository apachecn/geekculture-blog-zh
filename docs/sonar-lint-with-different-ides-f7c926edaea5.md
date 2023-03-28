# 不同 ide 的声纳线头

> 原文：<https://medium.com/geekculture/sonar-lint-with-different-ides-f7c926edaea5?source=collection_archive---------15----------------------->

获得编写更好代码的能力

![](img/b31faf1126850943b2ade1d059009b40.png)

**什么是声纳线头？**

Sonar Lint 是一个对代码进行静态分析的开源工具。使用这个工具，我们可以在开发过程中获得关于代码质量问题的即时反馈。这允许我们在潜在的错误和漏洞被提交到代码库之前解决它们。

Sonar Lint 支持的语言包括 C、C++、HTML、Java、JavaScript、PHP、Python 和 TypeScript。

让我们看看如何将 sonar lint 添加到智能智能中。

1.  打开 IDE 并选择`File -> Settings`

![](img/1f8c56ed7acc76ae99c4056548c1f2e3.png)

2.选择**设置**后，将出现以下窗口。在搜索栏中选择**插件**并输入**声纳**，点击**安装**。

![](img/90eede671941c5711efef5690eedf1f9.png)

3.然后点击**重启 IDE** 。

![](img/302cafc85dca97065ed220994e493fcd.png)

4.之后，如下图所示，你将能够在任务栏中看到 S**on print**。

![](img/a23dd2ba4853421f8a261fae8faf8bac.png)

**让我们看看如何将 sonar lint 添加到 NetBeans IDE 中。**

1.  打开 IDE 并选择顶部菜单中的工具。

![](img/b4fe10df5b8a27de620ac8c04a893dd2.png)

2.然后在下拉列表中选择**插件**。

![](img/e2c85c693ea301787f637263ae09d577.png)

3.然后这个插件弹出窗口会出现，你可以在搜索栏中选择**可用插件**和**T30**声纳**。如果您使用 13.0 或更高版本的 netbeans 版本和 11 或更高版本的 Java，您将能够看到 **sonarlint4netbeans** 。**

![](img/4147747d34b1a0bcb4ea0c2c9262bfeb.png)

4.如果出现，选择复选框，点击**安装**，然后点击**下一步。**

**注意:**如果您在安装时遇到任何代理问题。点击`General`选项卡中的`Tools -> Options` ，选择`No Proxy`单选按钮。

![](img/e2d46705a2dbe95944d82331931e51e0.png)![](img/2fcc31b18e2a5c2ecc7f0614a0a1c6ff.png)

连续点击**下一个**后，会出现如下窗口。根据需要选择重启选项，然后点击**完成**。

![](img/e944f37ed324693d51733d388b318b30.png)

毕竟，您可以通过选择项目，右键单击并选择选项“ **Analyze with SonarLint** ”来分析您的项目。

![](img/253dd7ba1ecd441789f13eec33a07d60.png)

**让我们看看如何在 Visual Studio 代码中添加 sonar lint。**

1.  打开 IDE 并选择`View -> Extensions`

![](img/6e8e75a303112a2a7178ce3a661f0b12.png)

2.选择**扩展**后，将出现以下窗口。在搜索栏中输入**声纳**并点击**安装**。

![](img/bcf68c00f8366da697779e3183f9e2c9.png)

**让我们来看看如何给 Eclipse 添加 sonar lint。**

1.  打开 IDE 并选择`Help -> Eclipse Marketplace`

![](img/78d5628d95d7b9b63d41f174f9514253.png)

2.选择 **Eclipse Marketplace…** 后，将出现以下窗口。在搜索栏中输入**声纳**，点击**安装**。

![](img/2bbcbc587097ba636ebd2ba31889699d.png)

3.**重启**IDE。

感谢您的阅读！希望文章有帮助。

**参考文献**

[1][https://www.sonarlint.org/](https://www.sonarlint.org/)

[2][https://dev blogs . Microsoft . com/premier-developer/real-time-code-quality-with-sonarlint-in-visual-studio/](https://devblogs.microsoft.com/premier-developer/real-time-code-quality-with-sonarlint-in-visual-studio/)