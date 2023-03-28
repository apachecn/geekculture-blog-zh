# 如何创建、测试和发布一个简单的 Gradle 插件

> 原文：<https://medium.com/geekculture/how-to-create-test-and-publish-a-simple-gradle-plugin-544ced1703f6?source=collection_archive---------14----------------------->

![](img/1e0acb03514a29c2dd475dca2408e9ab.png)

[https://gradle.com](https://gradle.com)

这个星期我不得不创建一个简单的 Gradle 插件，只是为了了解在这样一个插件中所有的东西是如何一起工作的。在我们公司，我们有专门的模板，但在这篇文章中，我会告诉你如何从头开始创建没有任何模板的插件。

# 为什么我们应该创建插件

如果一个项目变得越来越大，经过一段时间后，将逻辑的某些部分重新定位到一个插件中是一个好主意。这具有清理主项目的效果，更重要的是，它使逻辑可用于其他项目。这样，我们可以最小化不同项目中的代码冗余。现在，我们可以简单地应用带有所需逻辑的插件，而不必将相同的片段一遍又一遍地复制粘贴到不同的项目中。

# 如何创建插件

## 要求:

*   支持 Kotlin 和 Gradle 的 IDE
*   科特林
*   格拉德勒
*   JDK 1.8 或更高

首先，我们可以用我们选择的 IDE 创建一个新的 Gradle 项目。(我的是 Intellij IDEA。)现在我们可以向项目添加一些文件了。结构应该是这样的:

```
/src
---|/main/kotlin/org.example/HelloWorldPlugin.kt---|/test/kotlin/org.example/HelloWorldPluginTest.kt
```

然后我们可以编辑根 build.gradle.kts 文件。

这个文件与普通的 build.gradle 文件有三个主要的不同之处。

1.  需要`java-gradle-plugin`来允许插件的配置。
2.  `gradlePlugin{}`通过给插件一个 ID 并设置 implementationClass 来配置插件本身。实现类必须是实现插件<项目>接口的类。
3.  `gradleTestKit()`依赖增加了用测试项目测试插件的功能。

## 添加插件功能

打开 HelloWorldPlugin.kt 文件，向其中添加以下行。

首先，我们将看一看 HelloWorldPlugin 类。

它实现了插件接口，允许我们覆盖应用函数。在这个函数中，我们可以定义插件在应用时要做的任何事情。在这个函数中，我们还可以创建一个新任务，并为其添加功能。

第二部分是扩展类。它可以帮助用户配置插件以及它应该如何运行。例如，在这种情况下，该类有一个 message 属性，用户可以稍后在 build.gradle 文件中定义它。这使得插件的使用范围更广，并且不再需要硬编码的值。

现在这个插件实际上已经可以使用了。我们可以通过创建一个应用插件的新项目来测试这个插件。

# 测试插件

为了测试插件，我们必须创建一个测试项目来应用插件。Gradle 通过 gradleTestKit 依赖项提供此功能。我们可以在 HelloWorldPluginTest.kt 文件中添加以下行。

首先，我们必须创建一个 tempDir，其中包含 build- and settings.gradle.kts 文件。设置文件可以留空，但是构建文件需要填充一些内容。我们添加我们的插件并用消息属性配置扩展。

最重要的部分是梯度转轮。我们将 GradleRunner 的输出保存到一个结果变量。在那里我们必须配置 projectDir 和 Gradle 应该执行的任务。对于这些，我们使用 tempDir 和之前在插件中创建的任务。

最后一部分只是测试输出是否包含扩展中配置的字符串，以及任务输出是否等于“成功”。

# 发布插件

要发布插件，我们必须在 build.gradle 文件中修改和添加一些东西。

第一件事是将 gradle 的 plugin-publish 插件添加到我们的构建中。之后，我们可以用我们的项目网站、VCS 和一些我们想要显示的标签来配置 pluginBundle。此外，我们必须将显示名称和插件描述添加到插件配置中。

为了最终能够将我们的插件发布到 [Gradle Plugins Portal](https://plugins.gradle.org) 上，我们必须给自己弄一个 API-key，我们可以通过在[这个页面](https://plugins.gradle.org/user/register)上注册来获得。

我们现在可以通过运行以下命令来发布我们的插件:

```
$ ./gradlew publishPlugins -Pgradle.publish.key=<key> \
-Pgradle.publish.secret=<secret>
```

# 反射

## 什么进展顺利

Gradle 的文档非常好，里面有很多例子。让我的第一个插件工作起来很容易。我能够很容易地在主项目和 BuildSrc 目录中创建一个插件。

## 什么需要改进

由于未经测试的插件在现实世界中毫无用处，我需要为我的插件编写测试。首先，我在 GradleTestKit 上遇到了一些问题，我真的不知道在哪里应用这个插件。在一些架构变化之后，我能够在根项目中测试插件了。我在测试另一个子项目的插件时也遇到了问题。