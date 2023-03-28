# 我制造了一个人工智能来和我的朋友聊天，而不让他们察觉。(第一部分)

> 原文：<https://medium.com/geekculture/i-made-an-ai-to-chats-with-my-friends-without-them-noticing-part-1-c9a6482a4d80?source=collection_archive---------3----------------------->

![](img/c76e97adbc9d613728e036b0cbb2177e.png)

[DALL-E 2](https://openai.com/dall-e-2/)

所以，前几天我和我的一个好朋友聊天。然后，我突然想到一个主意，让我们制造一个可以和我的朋友聊天的人工智能，然后它可以用它实际上是一个人工智能来让他们吃惊。

所以，我知道了我在谈话中需要用到的东西。Ofc GPT-3 或 GPT-J 我只能从两者中选择一个。最后我决定用 GPT-3。

现在，我需要想办法从 whatsapp 上获取聊天记录。我调查了 whatsapp API。它是👎🏻。所以，我想到了一个替代方案。

![](img/65b2069f0538f8c41792883ce3b0c879.png)

This is the basic WhatsApp interface

![](img/57d233c4b4d20fedffe65a8b1cb8c220.png)

This is the area of most recent message in a chat with exact co ordinates of (501,778,1062,827)

现在，我们可以在这张图像上使用 tesseract OCR 和 OpenCV 从 WhatsApp 获取文本，然后这些文本可以在 GPT-3 上得到提示。

我将如何获得聊天？？

这里是如何

首先，我将截取特定区域的截图:

![](img/9873b56ebbe684c5222f67ee765450ed.png)

然后我会把图像变成负片:

![](img/4ee0335f6a85e58d12177cf1eec5e43e.png)

然后，我将检测单词并用 tesseract ocr 读取，并将文本粘贴到 word 文件中:

![](img/f5f78df0b6482bf9e18f1f81630e23a2.png)

现在我们有了文本，它可以通过 GPT-3 和答案可以发送到聊天。

**现在让我们从代码开始:**

首先让我们来看看导入:

让我们来截图:

![](img/600ea4cbaba0ea32ed4bf78b94d553df.png)

The Output

让我们设置 OCR:

让我们对图像进行预处理，并为 tesseract OCR 做好准备:

![](img/01ec48680362ff7e35697d27ea01914b.png)

Negative Image

让我们使用 tesseract ocr 和 opencv 来检测文本:

现在让我们将检测到的文本添加到. txt 文件中:

这就是获取数据部分的结束。

让我们清理数据，使其易于使用:

这段代码主要做的是从检测到的文本中删除时间戳。

现在数据部分结束了。

**让我们从人工智能部分开始。**

首先让我们获取导入:

让我们设置我们的 openAI API 密钥:

现在让我们准备好提示:

现在让我们设置 GPT-3 并微调它:

让我们从变量中提取响应并显示给用户:

现在让我们创建回复机制，该机制将基本上键入响应并按 ente:

现在我们需要删除 recognized.txt 文件中的所有文本:

现在，让我们制作一个 kill 开关，如果有奇怪的事情发生(比如发垃圾邮件)，它就会杀死这个程序:

这样一来，版本 1 的代码就完成了。

让我们看看一些输出:

![](img/71130542d2239d871d693032b3fc9a7c.png)

正如你可以看到它的垃圾邮件很多，因为它的速度非常快。这就是我们将在下一部分解决的问题。这个人工智能的改进将在那篇文章中完成，现在信息读取人工智能正在工作，并且正在给出响应。

[**感谢阅读。如果你喜欢这篇文章，请关注我。**](http://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)

有关代码，请点击此处:

[](https://github.com/Eeman1113/Chatterboii69) [## GitHub - Eeman1113/Chatterboii69

github.com](https://github.com/Eeman1113/Chatterboii69) 

关于我的每日 AIML 更新，请点击此处:

感谢阅读😁，下周见👋🏼。