# 和声状态:音频案例研究

> 原文：<https://medium.com/geekculture/the-harmonic-state-audio-case-study-ba0b3e8f7fff?source=collection_archive---------68----------------------->

交互式网络体验的网络音序器

![](img/d57226110a584f1bf43b8a6cf9f2b0fe.png)

沃森是 IBM 的商业人工智能系统。与 GPJ 和 ActiveTheory 一起，我们在 Plan8 有机会创造一个互动游戏体验来展示沃森的能力。

# 实时修改音乐

该网站的核心概念是展示沃森如何帮助企业在混乱中恢复秩序。随着游戏的进行，互动的视觉和声音应该反映出这种变化。每一关都以不和谐开始，以和谐结束，用户收集代表沃森洞察力的球体。

![](img/bf1df6884066bf67ca0f7cb07d5f307c.png)

我们建立了一个音乐参数的列表，我们认为在这两种状态之间进行转换会很有趣。

对于一些过渡，我们可以使用实时效果，如过滤和混响。但是对于其他的，像“失谐”和“非结构化”，我们不得不影响单个样本的回放。这意味着我们不能使用预先渲染的轨道。

在建立用户自己创作音乐的 beat maker 网站时，我们通常使用 web sequencers。对于体验网站和游戏，我们主要使用预渲染的轨道或音序器，如小节或第四个音符。这一次，我们需要将每个音符分别导出，以便能够实时改变时间和音高。

# 用收割机控制音调

多年来，我们一直试图找出在处理交互式音频项目时，导出和导入音频文件的最智能的管道。一个大项目可能有数百个文件需要从 DAW 导出、转换、更新并导入到项目构建中。手动操作会耗费大量的时间，并且经常会导致错误和混乱。

我们最近在作曲时转而使用 Reaper 作为我们的主要工具。与其他 Daw 相比，Reaper 的一个很大的区别是它可以用定制脚本进行扩展。

当我们开始这个项目时，我们研究了如何直接从 Reaper 导出关于音乐安排的信息。我们想要导出的信息是使用了哪些声音文件、*它们应该在传输时间线上的什么地方播放以及*声音文件如何在文件夹轨道中分组。**

我们最终创建了一个定制的 reaper 动作，将信息输出到一个 json 文件中。python 脚本使用了 [Reapy](https://pypi.org/project/python-reapy/) ，它是 [ReaScript Python API](https://www.reaper.fm/sdk/reascript/reascripthelp.html#p) 的包装器。这与 Reaper 的“区域渲染矩阵”相结合，使您可以轻松地将所有区域渲染为单独的文件，使 Reaper 成为交互式音频制作的最佳选择。

下一步是使用导出的 json 设置一个音序器在浏览器中播放。使用 web audio api 框架 [tone.js](https://tonejs.github.io/) 这是一项简单的任务。Tone 的[部分](https://tonejs.github.io/docs/14.7.77/Part.html)组件做的正是我们想要的，可以使用从 Reaper 导出的定时在正确的时间播放每个样本。

![](img/8dc550b032696f43f65d0a5f9b0f7c91.png)

*Changes in the Reaper project instantly updates in the web sequencer after running the custom action.*

# 将游戏与音乐同步

简报指出音乐应该在游戏中扮演重要的角色。所以我们想让你在游戏中抓到的球在节拍同步中出现在捕手面前。这是一项复杂的任务，因为游戏使用可变的时间刻度来控制游戏的速度。用户还可以选择在任何时候放慢游戏速度，以慢动作播放。

幸运的是，我们在音序器中把音乐分解成了十六分音符。这使得我们可以直接将游戏的时间表与序列 bpm 联系起来。结果是音乐轨道紧紧跟随游戏，使球体与节拍保持同步。

随着音乐在这个粒度音序器中播放，我们还可以独立于速度影响音高。我们在游戏中使用了这一点，开始时音乐稍微降低音调，然后随着游戏的发展慢慢地将音调升高到正常。

Transition from Dissonance to Harmony using tempo ramp and pitch shift of individual instruments

# 关于计划 8

Plan8 是一家为品牌、产品和体验创造音乐和声音的设计机构。Plan8 在音频、新兴技术、品牌和艺术的交叉领域运营。我们的团队由音乐和声音设计师、技术专家和音频战略家组成。成立于 2008 年。我们在斯德哥尔摩和洛杉矶设有办公室，并在全球范围内开展工作。
在 IG:[@ plan 8 music](http://instagram.com/plan8music)WEB:[www . plan 8 . se](http://www.plan8.se/)上关注我们的工作