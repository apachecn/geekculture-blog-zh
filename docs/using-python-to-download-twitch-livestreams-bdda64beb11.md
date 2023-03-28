# 使用 Python 和 Streamlink 下载 Twitch 直播流

> 原文：<https://medium.com/geekculture/using-python-to-download-twitch-livestreams-bdda64beb11?source=collection_archive---------15----------------------->

![](img/461d468ef5a2d886911fa661638812f5.png)

Photo by [Stanley Li](https://unsplash.com/@djravine?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 简介+先决条件

想要实时下载 Twitch 流，而不是等待整个记录可用吗？你是一名编辑，想要加快从你的流创建 YouTube 内容和汇编的周转时间吗？

这将是一个快速而简单的 Python 项目，有很多用例以及可以在其上构建的方法。让我们从您开始工作所需的先决条件开始:

1.  [安装 Python](https://www.python.org/downloads/) :任何版本≥ 3.8
2.  用 pip3 安装[Streamlink](https://streamlink.github.io/):**pip 3 install ' Streamlink>= 2 . 2 . 0 '**
3.  安装 [ffmpeg](https://ffmpeg.org/)

现在，我们所有的依赖项都已安装完毕，准备就绪。

接下来，我们需要向 Twitch 注册我们的应用程序，以获得客户端 ID 和密码。前往 [Twitch 开发者控制台](https://dev.twitch.tv/console/apps)并注册一个新的应用程序。一旦完成，你现在应该有一个 ID 和秘密来使用。

# Twitch 和 Streamlink 的 Python 包装器

感谢开源和 GitHub 的力量，我们不需要为使用 Twitch API 和 Streamlink 编写自己的 Python 包装器。我们基本上将使用这个[存储库](https://github.com/ancalentari/twitch-stream-recorder)中的脚本，并添加我们自己的配置。我在编写自己的 Python 脚本时发现了这个库。它已经写得很好了，也记录在案了，所以我放弃了我的。继续克隆它，并给它一颗星:)。

在 **config.py** 中，添加你想要下载直播的路径，你想要录制的人的 Twitch 用户名，以及你的客户端 ID 和 secret。

如果我们看一看 **twitch-recorder.py** ，我们会看到该脚本获取配置文件并检查我们的客户端凭证。它还会在启动 Streamlink 开始下载之前检查用户名的流是否在线。

如果我们按照自述文件运行:

```
python3 twitch-recorder.py
```

这应该是我们开始录音所需要的。注意，如果这不起作用，请返回到上一节，确保所有的依赖项和 Twitch 凭证都是正确的。就我个人而言，我需要将 Streamlink 升级到 2.2.0 版本，以便与 Twitch 一起使用。

# 后续步骤

我们创建的 Python 脚本非常简单。它只是用给定的参数启动一个 Streamlink 记录过程。然而，因为我们现在有了 Streamlink 的 Python 包装器，所以我们可以用各种方式扩展这个脚本。我将让您自己来实现这些，但是这里有一个您可以尝试的列表:

## 与 Twitch API 集成

与 [Twitch API](https://dev.twitch.tv/docs/api/) 集成可以让你做很多很酷的事情，比如:

*   当特定的流媒体工具上线时，请监听
*   收听流媒体工具何时更改流类别/标题
*   阅读 Twitch 聊天

通过订阅频道事件，如频道何时上线和何时离线，您可以在正确的时间开始录制直播流，不会错过任何部分。我们还可以选择仅在流媒体工具播放特定类别(如“只是聊天”)时进行录制。这将需要服务器或始终运行的脚本来订阅频道事件。

## 一次录制多个流

如果您是多个通道的编辑者，或者您只是对多个通道感兴趣，那么您可以通过修改 Python 脚本来一次记录多个流，以便为每个所需的流一次生成多个 Streamlink 记录过程。这样做对于为多人游戏编译多个视点来说很酷。例如，像 GTA 这样的游戏，其中几个流在同一个服务器上跳跃，并一起进行角色扮演，这将是一个很好的用例。

## 共享录像

如果您是编辑或内容创建者团队的一员，您可以设置一个文件共享服务器并在那里录制流，这样每个人都可以访问它们，而无需自己录制。

## 视频处理

为了进一步自动化您的编辑过程，请尝试使用 Python 来执行一些简单的编辑任务。 [MoviePy](https://pypi.org/project/moviepy/) 是一个 Python 库，用于剪切等常见编辑任务。每当流标题发生变化时，这可以用于诸如剪切视频之类的事情。

# 结论

总之，我们能够使用 Python 和 Streamlink 实时记录来自 Twitch 的直播流。这有很多应用程序，有助于加快内容创建和编辑过程。希望你从这个简短的项目中学到了一些东西，并受到启发，用它来创建新的东西！