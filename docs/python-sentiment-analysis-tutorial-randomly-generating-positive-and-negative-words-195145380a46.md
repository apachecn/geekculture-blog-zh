# Python 情感分析教程:随机生成“正面”和“负面”单词

> 原文：<https://medium.com/geekculture/python-sentiment-analysis-tutorial-randomly-generating-positive-and-negative-words-195145380a46?source=collection_archive---------10----------------------->

![](img/fd94e714c218a41a58d966de0839e647.png)

Image by Arek Socha from Pixabay

随着**机器学习**继续在技术领域表现出色，机器学习的一个重要子主题是**情感分析。**

# **情绪分析**

情感分析是分类工具的应用，它将确定一段文本的“情感”——在某些情况下也可能是非文学文本。

# 情感分析的重要性

情感分析对于企业监控客户反馈、艺术家检查反馈、政治家了解公民在线反应等都非常重要。

此外，情感分析是社交媒体营销活动的一个重要因素。通过丢弃来自脸书、Twitter、Instagram 和其他社交媒体平台的数据，这些活动的运营者可以推断出个人对这些产品或服务的反应。因此，社交媒体活动将基于所述响应进行修改，以更好地符合大多数人的需求。

# **情感分析示例**

情感分析有各种各样的用法。一种是基于顾客和客户的反应。例如，“我真的很喜欢这个产品！”会产生“积极情绪”相反，“这个产品很糟糕”会产生“负面情绪”另一种情感形式是“中性情感”例如，“我不能决定我是喜欢还是讨厌这个产品。”

在这个项目中，我们不会测试“中性情绪”这是因为默认情况下大多数单词都是中性的。例如，书、房子、汽车等。

# 使用 Jupyter 笔记本进行分析

**简介—**

*   该分析的第一步是导入并下载“随机单词”库。
*   接下来，需要导入并应用“自然语言工具包”。
*   需要的最后一个库是“Vader 情绪”,用于测试一个单词或短语的情绪。

如果用户还没有下载“random-word ”,那么要启动笔记本电脑——它也可以在普通的控制台上运行，只需进行一些重组。

```
pip install random-word
```

接下来需要导入 RandomWords，可以看到如下。

```
from random_word import RandomWords
```

接下来，我们必须对“自然语言工具包”(nltk)做同样的事情。

```
pip install nltk
```

类似地，现在我们必须导入工具箱。

```
import nltk
```

可以随机生成单词的方法如下:

```
r = RandomWords()randomOne = r.get_random_word()
randomTwo = r.get_random_word()
randomThree = r.get_random_word()print(randomOne, randomTwo, randomThree + ".")
```

在这个例子中，打印将返回*“无热量 genty。”*

接下来，我们需要安装“维德情绪”这将以类似于前两者的方式完成。

```
pip install vadersentiment
```

然后，我们需要引入“维德情绪”

```
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
```

在这个程序中，我们会提示用户，如果他们想随机生成一个词，积极或消极的情绪。

这两个功能是“积极随机”和“消极随机”

它们的编写如下所示。

```
def positiveRandom(running):

    while (running == "True"):

        randomValue = r.get_random_word() print(randomValue) sentence = randomValue vs = SentimentIntensityAnalyzer().polarity_scores(sentence) for i in vs: if (i == "pos"): if (vs[i] > 0): print("\nPositive Word: " + sentence)

                 return(sentence) running = "False"
```

```
def negativeRandom(running):

    while(running == "True"):

       randomValue = r.get_random_word() print(randomValue) sentence = randomValue vs = SentimentIntensityAnalyzer().polarity_scores(sentence) for i in vs: if (i == "neg"): if (vs[i] > 0): print("\nNegative Word: " + sentence)

             return(sentence)               

             running = "False"
```

在上面两个函数中，两者的结构是相似的。

while 循环的目的是继续循环，直到根据函数找到一个正的或负的单词。

单词“vs”的初始化将用于保存随机生成的单词的情感。

在初始化过程中，“vs”将保存多个值。因此，我们需要根据我们要寻找的东西找到“正”或“负”。

默认情况下，“正”和“负”等于 0。因此，如果单词将返回大于 0 的值，则情绪可以被确定为负面的或正面的。

*   找到后，while 循环将停止运行，然后返回单词。

```
#Take it out step further, randomly generate words until it's positive or negative based on the user's choice!value = ""running = "True"userInput = input("Would you rather generate a word with positive or negative sentiment? Type 'P' or 'N'. ") 

if (userInput.lower() == "p"):

    value = positiveRandom("True")

elif (userInput.lower() == "n"):

    value = negativeRandom("True")
```

上面的函数用于询问用户他们是否想要生成一个“肯定的”或“否定的”单词。

# **结论**

在这篇文章中，应用了情感分析方法。可以看出，情感分析有多种应用。然而，在这篇文章中，使用了“维达情绪”，并且不需要“训练”和“测试”数据集。通常要做一个情感分析功能，两者都需要。然而，可以看出，虽然不理想，“维德情绪”是一个适用的捷径。

# 开源代码库

GitHub 文件的链接如下:

[链接](https://github.com/AliFakhry/PythonAI/blob/main/NLP/NLPMedium.ipynb)。