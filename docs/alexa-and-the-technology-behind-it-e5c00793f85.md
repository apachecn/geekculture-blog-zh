# ALEXA 及其背后的技术

> 原文：<https://medium.com/geekculture/alexa-and-the-technology-behind-it-e5c00793f85?source=collection_archive---------7----------------------->

![](img/fa60c55c8fe5d377a70816b72d853373.png)

**什么是 Alexa？**

Alexa 是亚马逊基于自然语言处理的系统。Alexa 是亚马逊 Echo、Dot、Tap、FireTV 和其他第三方产品(有 100 多个)中的虚拟助手。这项技术于 2012 年首次推出，现在已经成为我们生活中不可或缺的一部分。从孩子找到了可以帮助他们做作业的人，到依靠 Alexa 作为可靠伙伴提醒他们日常家务的老年人。对于年轻的专业人士和忙碌的妈妈们来说，只需使用语音命令，就可以自由地制作清单、控制智能家居等。它不再仅仅是一个天气预报或音乐播放设备，而是一个超过 10 万种技能的仓库。

【Alexa 是如何工作的？

*Alexa，《打开厨房的灯》*。在这种情况下，厨房灯是一个智能灯泡/灯，支持 wifi，可以由 Alexa 控制。飞利浦 Hue 和 LIFX 灯泡可以用 Alexa 控制。这是一个简单的命令，结果也很简单，我们看到*‘厨房灯打开’*，但后台发生的事情远不简单。

底层硬件设备(如 Amazon Echo)通过其麦克风录制语音。底层设备始终连接到互联网。录制的语音被发送到 Alexa 语音服务(AVS)。AVS 理解录音并将其转换为命令。例如，在这种情况下*打开请求‘厨房灯’*。然后，系统在已注册的智能家居设备列表中搜索*“厨房灯”*及其附属技能。然后，在这种情况下，它通过打开标记为*“厨房灯”*的智能灯，将相关输出发送回设备。

![](img/e2038532eb36e9f4350f76cf269edb2c.png)

因此，让我们更深入一点，了解后台的技术流程。Alexa 背后的主要技术或概念是自然语言处理(NLP)。NLP 帮助机器理解和处理人类语言。它将文本转换成结构化数据。让我们把它进一步分解成更小的概念。

**信号处理**

信号处理的目标是增强输入信号，在这种情况下是语音，并最小化背景噪声，以便我们能够更好地理解输入信号。它专注于分析、修改和合成信号。它通过净化信号来帮助设备理解音频。

**唤醒字检测**

在这种情况下，唤醒词是*‘Alexa’*。设备需要唤醒词来理解我们正在与它对话。正如我们所知，这些设备总是连接到互联网上，并监听我们的谈话，一旦他们听到唤醒词，他们就被激活。想象一个场景，没有唤醒词，Alexa 是你所有对话的一部分，唉！它将导致的混乱。

一旦检测到唤醒词，信号将被发送到亚马逊语音服务，该服务将音频转换成文本格式。通过麦克风记录的声音被转换成二进制数据，音频的频谱图被用来识别所说的话。

**自然语言理解**

一旦说出的单词被识别，理解单词被说出的顺序也是很重要的。例如*【播放提醒我】*和*【提醒我播放】有很大区别。*自然语言理解(NLU)将文本转换成有意义的表示。它超越了单词的识别，有助于确定意图。

**自然语言生成**

一旦确定了命令的意图和正确顺序，智能灯泡技能将被激活，并通过互联网打开*【厨房灯】*。然后，Alexa 将使用自然语言生成(NLG)响应“*厨房灯已打开”*。NLG 将结构化数据转换成模拟人类对话的文本。

希望这些信息能帮助你下次在一群极客中展开一场智慧的对话。

如果你是一名有抱负或已经成名的数据科学家/数据分析/人工智能爱好者，请在 [Instagram](https://www.instagram.com/commonlytics/) 上关注我们。

参考资料:

[](https://towardsdatascience.com/how-amazon-alexa-works-your-guide-to-natural-language-processing-ai-7506004709d3) [## 亚马逊 Alexa 是如何工作的？你的自然语言处理指南

### 我们现在可以与几乎所有的智能设备对话，但它是如何工作的呢？当你问“这是什么歌？”，什么…

towardsdatascience.com](https://towardsdatascience.com/how-amazon-alexa-works-your-guide-to-natural-language-processing-ai-7506004709d3) 

[https://www . Forbes . com/sites/bernardmarr/2018/10/05/how-does-amazons-Alexa-really-work/？sh=4fc0ffb71937](https://www.forbes.com/sites/bernardmarr/2018/10/05/how-does-amazons-alexa-really-work/?sh=4fc0ffb71937)

[https://towards data science . com/NLP-vs-nlu-vs-NLG-know-what-you-that-to-achieve-NLP-engine-part-1-1487 a2 c8 b 696](https://towardsdatascience.com/nlp-vs-nlu-vs-nlg-know-what-you-are-trying-to-achieve-nlp-engine-part-1-1487a2c8b696)

[https://www . about Amazon . in/news/devices/customers-in-India-say-I-love-you-to-Alexa-19-000 times-a-day](https://www.aboutamazon.in/news/devices/customers-in-india-say-i-love-you-to-alexa-19-000-times-a-day)