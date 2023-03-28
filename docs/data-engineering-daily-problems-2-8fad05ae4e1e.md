# 数据工程日常问题-2

> 原文：<https://medium.com/geekculture/data-engineering-daily-problems-2-8fad05ae4e1e?source=collection_archive---------7----------------------->

使用这些双倍定量的用例来提高您的隐含知识:自定义 URL 生成器和压缩 XLSX 文件到 Spark 表的转换。Python 中的代码示例。

![](img/891d8b7c05497b211b6cc7b8563af405.png)

[Created with Dall-E](https://labs.openai.com/s/MbZN20PZGbdjPjZXZR4gPu3H) — Image by author

# 介绍

本文是该系列的第二部分，始于:

[](/mlearning-ai/data-engineering-daily-problems-1-85ca36192478) [## 数据工程日常问题-1

### 提高你的隐性知识与这些双重定量的闹鬼使用自动气象站λ

medium.com](/mlearning-ai/data-engineering-daily-problems-1-85ca36192478) 

我鼓励你阅读上一篇文章中对该系列的介绍，但如果你懒到想要保存一次点击，让我给你总结一下:

> 我将开始汇编我在作为数据工程师的日常工作中遇到的一系列问题，这些问题作为单独的解决方案可能没有价值(如果你没有直接遇到和我一样的问题)，但可以帮助开发该领域的隐含知识。
> 
> 我的想法是在这一系列文章中汇编这些例子，以便在最好的情况下，它们可以帮助面临相同问题的人，在一般情况下，它们可以作为好奇心或帮助开发隐性知识。

这篇文章的许多问题都与不得不从互联网上下载文件有关。在第一个示例中，我们必须推断下载文件的可能 URL，在第二个示例中，我们必须处理一个包含几个 XLSX 文件的 ZIP 文件，这样每个工作表都可以用于在 Spark metastore 中创建一个表。

# 问题

## 问题 3: URL 生成器

**描述**

从一个 PDF 文件中下载和提取文本，该文件每周由一个人生成，并在一个未知的 URL 中发布。这个文件遵循一个命名约定，但是很明显它是由一个人创建，因为命名约定只是部分地被遵循。一些注意事项:

*   我们不知道文件将在哪天上传:URL 中文件的日期不一定是上传的日期(4 天范围)。
*   在正常的 TDD 中，当一个新的 bug 被识别时，用于识别该 bug 的代码必须被添加到我们的测试池中。这个案例采用了类似的方法:当一个新的 URL 没有被我们的流程识别时，构建该 URL 格式的案例必须被添加到我们的代码中，这就是为什么我们需要一个可扩展的流程(因为搜索到的 URL 可能是应用若干转换的结果)。

**例句**:

```
web.es/f/files/Press%20notes/PRESS_NOTE_DATE_27_9_20.pdf
web.es/f/files/Press%20notes/PRESS_NOTE_DATE_30_09_20.pdf
web.es/f/files/Press%20notes/PRESS_NOTE_DATE_14_02_2021.pdf
web.es/f/files/Press%20notes/PRESS_NOTE_DATE_30_9_2021.pdf
web.es/f/files/Press%20notes/PRESS_NOTE_DATE_2_7_2021.pdf
web.es/f/files/Press%20notes/PRESS_NOTE_DATE_2_07_2021.pdf
```

我们已经确定了 6 种不同的日期格式，但是如果我们确定另一种日期格式，比如:

```
web.es/f/files/Press%20notes/PRESS_NOTE_DATE__23_03_22.pdf
```

对我们可能面临的所有不同的 case 条件进行编码将是一场噩梦，此外，每次我们想要生成一个新的可以与其他条件组合的 casuistic(就像前面的例子)，我们都必须重复大量的代码，我们的程序将变得难以管理。

**解决方案**

组合使用: [**itertools**](https://docs.python.org/3/library/itertools.html)

算法，为这种 PDF 文件生成可能的名称，并检查它们是否与文件本身匹配。

执行的主要是方法"***【search _ pdf _ and _ get _ URL】***，在那里执行的过程如下:

首先，生成可能的日期。我们之前说过有一个 4 天的范围。更具体地说，这一天的范围是(-1，3)，所以如果今天是 7 月 25 日，我们将搜索并尝试使用日期来推断 URL 月 24 日、7 月 25 日、7 月 26 日、7 月 27 日。这是正在计算中的方法“***get _ dates _ to _ check****”。*

然后，为前一天范围生成可能的 URL，首先我们必须生成可能的组合，为此，我们调用“***generate _ combinations****”，它将返回类似于:*的内容

```
[[0], [1], [2], [3], [1, 2], [1, 3], [2, 3], [1, 2, 3]]
```

一旦有了组合，就创建了可能的 URL，将每个数字分配给一个方法/功能(因为所有的转换可以同时应用于 URL，例如，在组合 *[1，2，3]* 中，从日[1]和月[2]中删除 0，然后将剩余的数字添加到年[3])。

当然， **itertools** 返回的组合可能是硬编码的，但是**如果我们想添加一种格式化日期的新方法呢？**然后，我们只需修改该方法的输入(数字=3 到数字=4，以生成所有可能的组合)，我们必须修改“***generate _ all _ URLs _ per _ fecha”***方法，为“4”的情况添加一个新的条件(例如:交换日期和月份的顺序)。

最后，使用 SET 对象中所有生成的 url(为了避免重复)，我们检查这些 URL 是否存在，为此，使用方法"***check _ if _ URL _ exist***"

这样做，我们坚持 [DRY 原则](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)，并且扩展我们算法的功能(这肯定会发生，因为新的案例依赖于人类)是一个更容易的任务。

给读者的问题:有人知道我在这里使用了哪种方法吗？

## 问题 4:将压缩的 XLSX 文件转换为 Spark 表

**描述**

下载一个包含 XLSX 文件的压缩文件，该文件的页数不确定。这些表中的每一个都必须转换成一个可以从 Spark metastore 引用的表。这些工作表并不总是类似于表格的格式(第一行并不总是标题)，所以我们必须以这样一种方式处理它们，即我们可以自动删除那些不必要的行。

**解决方案**

**熊猫**+[openpyxlT5【熊猫用来读取 Excel 文件的 Python 库】的使用。](https://openpyxl.readthedocs.io/en/stable/pandas.html)

*。zip* 文件从 URL 下载并解压缩。

一旦所有提取的文件都在同一个路径( *xlsx_path* )中，该路径上的 xlsx 文件的文件列表被存储在一个列表中，因为我们不确定压缩后的文件的数量。

Pandas 用于读取 Excel 文件，并且对于该 Excel 文件中的每个工作表，从文件的开头开始，搜索有效的标题(与文件的其余部分具有相同列数的行，换句话说，没有“*未修改的*”列的行)。Pandas 会用名称类似于“ *Unamed-X* ”的字段自动填充丢失的标题。

最后，对于每一个工作表，将创建一个新的 CSV 文件，它结合了该 CSV 文件所属的 Excel 文件的名称和工作表的名称(因此我们可以很容易地识别原始文件)。CSV 被存储，然后使用以下代码创建一个火花表:

```
CREATE TABLE table_name USING CSV OPTIONS ('header', 'true') LOCATION 'path_to_csv'
```

**另一种选择是使用**:

[](https://github.com/crealytics/spark-excel) [## GitHub - crealytics/spark-excel:通过 Apache POI 读取 excel 文件的 spark 插件

### 一个用 Apache Spark 查询 Excel 文件的库，用于 Spark SQL 和 DataFrames。由于个人和专业…

github.com](https://github.com/crealytics/spark-excel) 

**(仅适用于 Scala)** 一个 Spark 库，允许我们加载一个 Excel 文件(XLSX)作为 Dataframe。我用这个库做了一些测试，结果不如 Pandas+[**openpyxl**](https://openpyxl.readthedocs.io/en/stable/pandas.html)**，**好，但是也许对于其他更简单的用例，这个库已经足够了。

```
“The success formula: solve your own problems and freely share the solutions.”
― [**Naval Ravikant**](https://twitter.com/naval/status/1444741381579177984)
```

# 结论

在这篇文章中，我们看到:

*   问题 3:如何创建一个 URL 生成器来检查几种可能的命名约定。
*   **问题 4** :下载一个压缩文件，提取他们的 XLSX 文件，对于他们每个人，提取他们的工作表为 CSV 文件，处理它，用他们每个人创建一个火花表。

这类文章的目的不仅仅是揭示一个非常具体的问题的解决方案，而是通过向读者展示具体的问题和解决问题的过程来帮助发展隐含的知识。

```
**Want to Connect?**[@data_cyborg](https://twitter.com/data_cyborg)
[https://www.linkedin.com/in/ivan-gomez-arnedo/](https://www.linkedin.com/in/ivan-gomez-arnedo/)
```