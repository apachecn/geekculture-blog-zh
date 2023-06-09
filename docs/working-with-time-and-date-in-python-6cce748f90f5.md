# 在 Python 中处理时间和日期

> 原文：<https://medium.com/geekculture/working-with-time-and-date-in-python-6cce748f90f5?source=collection_archive---------11----------------------->

![](img/4fd647019ff356f621fa915b37b6e945.png)

Photo by [Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我经常要做的事情之一是处理包括时间在内的各种形式的数据。我认为这对于每个人来说都是有益的，因为有时改变你的时间或日期格式是强制性的。例如，您可能正在分析来自温度或湿度传感器的时间序列数据，或者您可能正在查看一些公司销售报告信息，或者您正在记录您的个人开支。用例实际上并不重要，您总是需要处理时间和日期来正确地表示在某个时间点发生的信息。因此，在这篇文章中，我将分享一些最常见的时间和日期操作，你可以在 Python 中完成。

# **获取当前时间和日期**

要开始在 python 中处理时间和日期，您需要将 datetime 模块导入到代码中:

```
import datetime
```

现在，你可以很容易地得到当前的时间和日期

```
current_time = datetime.datetime.now()
print(current_time)
```

在运行此示例的过程中，我收到了下面提供的响应:

**输出:**

```
2021-09-12 22:11:15.327779
```

如您所见，返回的响应格式为 ***年-月-日小时:分钟:秒。毫秒*。**需要注意的是 ***now()*** 方法返回一个时间的本地日期和时间，这样完全取决于你所在的位置。另外，您可以将一个 timezone 参数传递给****now()***方法，这样您就可以接收 UTC 格式的时间和日期。*

***输出:***

```
*2021-09-12 22:19:38.959631
2021-09-12 19:19:38.959631+00:00*
```

*正如你在我的例子中看到的，UTC 时间比我的本地时间少 3 个小时。*

*很多时候你不需要拥有毫秒级的信息，所以你可以直接使用***【split()】***的方法。需要注意的是，***datetime . datetime . now()****返回的对象是一个****datetime****类， *s* o 如果要应用***split()，就必须将其转换为一个字符串。*******

```
**current_time_without_ms = str(datetime.datetime.now()).split('.')[0]
print(current_time_without_ms)**
```

**在运行这个示例的过程中，我收到了下面提供的响应。**

****输出:****

```
**2021-09-12 22:30:17**
```

**在上面的代码示例中需要***【0】***的原因是因为应用后， ***split()，*** 响应将是一个字符串对象列表 *s.* 如本例所示，结果如下所示。**

****输出:****

```
**['2021-09-12 22:30:017', '193881']**
```

**所以我们只需要列表中的第一个元素。**

# ****检索单独的数据字段****

**现在使用同一个 ***current_time*** 对象，我们可以分别检索各个字段。我认为下面的代码是不言自明的。请注意，打印的结果使用 python f-string，这需要一些字符串格式才能很好地显示结果。下面提供了代码示例。**

****输出:****

```
**2021-09-13 18:06:04**
```

# **使用时间戳**

**你可能遇到的另一件事是 Unix 时间戳。您还可以听到一些不同的名称，其中一个很常见的名称是 Posix timestamp，但实际上意思是相同的。在物联网行业中，各种设备向服务器发送时间戳信息而不是纯日期和时间是很常见的，因为它占用更少的空间。所以 Unix 时间戳就是自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的秒数。知道这一点就足以处理 Python 中的时间戳了。**

**例如，您想将 UTC 时间 2021–09–10 16:54:23 转换为时间戳？**

****输出:****

```
**1631292863.0**
```

**现在，如果您想进行时间和日期的转换，可以使用***fromtimestamp()***方法:**

****输出:****

```
**2021-09-10 16:54:23+00:00**
```

**正如您所看到的，使用 datetime 模块处理时间戳非常简单。**

**实际上有一个很酷的网站，我在处理时间戳时经常使用。您可以将时间和日期转换成时间戳，反之亦然，还可以做更多的事情。你可以随时复查你的计算是否正确:[https://www.epochconverter.com/](https://www.epochconverter.com/)**

# **计算时差**

**另一件很常见的事情是，如果我们给时间对象减去或加上某种值，就可以找出过去的日期和时间(或者未来的日期和时间)。也就是说 ***timedelta()*** 方法开始起作用。比方说，你有一个特定的日期，例如***2021–08–10 17:45:00***并且你想知道，如果你加上或减去特定的天数、小时数等，那么这个日期和时间是未来的还是过去的。下面的代码示例演示了如何做到这一点。**

****输出:****

```
**2021-08-06 15:32:00
2021-08-12 21:07:00**
```

*****strptime()*** 方法用于从保存时间和日期信息的字符串创建 datetime 对象，在本例中为***start _ time _ and _ date***。**

**所以，如果你想知道确切的日期和时间，你可以这样做，但是如果你想知道两个日期或时间戳之间的差异，比如说天/小时/分钟/秒呢？这也可以实现。让我们从前面的例子中得到结果，并找出***2021–08–12 21:07:00***和***2021–08–06 15:32:00***之间的区别**

****输出:****

```
**6 days, 5:35:00**
```

**正如您所看到的，我们已经很容易地计算出两个 datetime 对象之间的差异。**

# **了解 strftime()和 strptime()方法**

**这里最后要说的就是这两种方法。它们在 datetime 模块中提供，其用途是方便地将 datetime 对象转换为字符串，反之亦然。现在我不打算深入探讨所有的格式化可能性，但是它们在 python 文档中有很好的描述，您可以在下面的文章中找到:**

 **[## 日期时间-基本日期和时间类型- Python 3.9.7 文档

### 模块只提供一个具体的类，即类。该类可以表示简单的时区，具有固定的…

docs.python.org](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes)** 

**无论如何，我会给你看几个例子。假设您需要创建一个包含日期字符串的 datetime 对象。现在，这些字符串可以有各种格式，这就是为什么你需要一个格式化字符串，以便日期时间模块可以正确解析你的输入。下面举例说明了 ***strptime()*** 方法的用法**

****输出:****

```
**2021-08-06 16:00:00
2021-08-06 16:00:00
2021-08-06 16:00:00
2021-08-06 16:00:00
2021-08-06 16:00:00**
```

**从示例中可以看出，所有情况下的输出都是相同的。这是因为我们向***strptime()****提供了日期和时间字符串的精确格式。***

**现在我们可以反过来使用与 ***strftime()*** 相同的逻辑，这将允许我们从 datetime 对象中获取一个字符串。为了更好地直观理解，我将使用相同的***2021–08–06 16:00:00***值**

****输出:****

```
**2021-08-06 16:00:00
2021/08/06 16/00/00
06 August 21 - 16:00:00
06 August 21 - 04 PM
Aug 2021, 06 / 04 PM**
```

**在我上面提供的文档链接中，你可以找到其他的格式化选项，但是我希望你能够了解这两种方法是如何工作的，以及何时如何使用它们。**

# **结论**

**在 python 中，处理时间和日期是一项非常常见的任务。本教程展示了理解如何使用 python 中的 datetime 模块来简化日常工作中处理时间序列数据的基础知识。**

**我已经将这里使用的所有例子上传到我的 github 库。有一点我想提一下，实际上这个模块叫做 datetime。但是使用的类也叫 datetime。这就是为什么在代码中你会看到**

```
**import datetime as dt
from datetime import datetime**
```

**这样做是为了更方便地访问 datetime 类方法，因此，例如，您可以编写***current _ time = datetime . datetime . now()***，而不是**

***希望这不会太令人困惑，但如果你有任何问题，我很乐意回答。***

***链接到 github 库，所有的例子都在一个地方:[https://github.com/vycart/datetime_tutorial](https://github.com/vycart/datetime_tutorial)***

***感谢您的阅读。***