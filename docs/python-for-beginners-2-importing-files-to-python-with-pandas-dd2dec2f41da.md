# Python 初学者#2 —用 pandas 将文件导入 python

> 原文：<https://medium.com/geekculture/python-for-beginners-2-importing-files-to-python-with-pandas-dd2dec2f41da?source=collection_archive---------25----------------------->

![](img/8c0b899f04bd80b57ef698566ed3f16f.png)

*用熊猫上传 CSV、TXT、Excel 文件*

# 开始前的故事时间

学习 Python 并不是一件容易的事情。但是一致性是达到提升你职业水平的关键。

我们经常听到千禧一代希望事情变得简单。事实上，有很多年轻的专业人士认为他们可以为公司做更多的事情，但他们在职业生涯开始时面临的工作文化阻碍了他们的发展。

我很幸运地在学业结束后找到了工作，我记得在开始新工作后不久，我立即感到一阵失望。我觉得自己就像一台巨大机器中的一个齿轮。除了一个“ ***资源*** ”之外，我其实什么也不是。每天额外的 8-15 个小时的人力，取决于我老板的突发奇想。

结果是最终的醒悟和缺乏动力，原因很简单，在很大程度上，我被期望保持安静，做好我的工作，希望有一天我足够高，能够实现重大的改变。虽然老一辈人通常会告诉我要振作起来，但我看不出自己会在 5 年或更长时间内振作起来。我知道我会变得陈腐，害怕改变，就像那些告诉我呆在我的地方的人一样。

对于任何处于类似情况的人来说， ***尽最大努力提高自己的技能*** 找到一个适合自己的环境。这就是这些文章的全部目的。让你走上自由之路。

# 介绍

在这次演示中，我将使用来自[这场卡格尔比赛](https://www.kaggle.com/c/titanic/data?select=train.csv)的数据。这是一个简单的 CSV 文件，包含泰坦尼克号上的个人数据和不同的个人资料，即(年龄，婚姻状况等。)

我想把这个文件导入 python。我将向您展示如何做到这一点，以及您可能遇到的所有可能的故障排除。

# 目录

1.  你应该把你的文件放在哪里？
2.  [**读取 CSV 和 TXT 文件**](#559d)
3.  [**读取 excel (XLSX)文件**](#6a43)

# 你应该把你的文件放在哪里？

如果你知道什么是工作目录，就跳过。

这可能是你需要从这篇文章中学到的最重要的东西。我几乎放弃了 Python 的工作，因为我不理解这个非常简单的概念。

## **工作目录**

要访问计算机上的文件，Python 通常指向单个文件夹。默认情况下，这应该在你的 ***C:/中的某个地方。*** 我们称这个文件夹为 ***工作目录，*** 本质上就是 Python 工作的地方。

所以这个文件夹(这个工作目录)中的任何文件都可以在 Python 上立即读取。

如果你想知道你的工作目录在哪里，你可以使用`os`包。`os`是 operating system 的简称，在 python 中用来高效管理文件和文件夹。

为此，我们首先导入包(不需要 pip 安装，因为这是 python 发行版附带的默认包。)

```
import os# Find the current working directory
os.getcwd()
```

## **在不改变工作目录的情况下获取文件位置**

如果你想得到一个文件的位置而不需要改变你的工作目录，你必须给出文件的完整位置。

我将 Kaggle 网站上的 train.csv 文件存储在一个名为 Class 3 的文件夹中。

因此，为了读取我的文件，我需要 pandas 函数的输入如下:

```
"/home/caleb/Desktop/PythonClasses/Class 3/train.csv"
```

或者在 Windows 上:

```
"C:/Users/caleb/Desktop/PythonClasses/Class 3/train.csv"
```

## **更改您的工作目录**

要改变你的工作目录，你需要从`os`包中调用`chdir`函数。 ***但是需要使用完整的地名。如果你在工作中使用网络驱动器，这一点尤其重要。***

我喜欢做的是将位置赋给一个变量，并在我的`chdir`函数中使用它。

所以在 Linux 上应该是这样的:

```
LOCATION = "/home/caleb/Desktop/PythonClasses/Class 3"
os.chdir(LOCATION)
```

如果您使用的是 Windows，应该是这样的:

```
LOCATION = ‘C:/Users/caleb/Desktop/PythonClasses/Class 3’
os.chdir(LOCATION)
```

注意:如果你在网络驱动器上，那么我倾向于使用双反斜杠(\\)以确保安全。

这里， ***网络名*** 和 ***网络子文件夹*** 为网络文件夹。在 Windows 上，你可以很容易地在你的文件浏览器的顶部找到这些。

**注意从斜杠到双反斜杠的变化**:

```
LOCATION = ‘\\\\NETWORKNAME\\NETWORKSUBFOLDER\\Folder\\SubFolder\\’
os.chdir(LOCATION)
```

# 读取 CSV 和 TXT 文件

既然我们已经讨论了工作目录，导入文件就容易多了。

在 Python 中，`read_csv`函数读取 CSV 和 TXT 文件。所以就像导入`pandas`运行一样简单:

```
import pandas as pd# Find the full location of the file
file_loc = "C:/Users/caleb/Desktop/PythonClasses/Class 3/train.csv"# Find the current working directory
training_data =  pd.read_csv(file_loc)
```

当然，如果您已经更改了工作目录，那么您可以使用:

```
import os
import pandas as pd# Find the full location of the working directory
LOCATION = "C:/Users/caleb/Desktop/PythonClasses/Class 3"# Change working directory
os.chdir(LOCATION)# Find the current working directory
training_data =  pd.read_csv("train.csv")
```

## 逗号中的不同分隔符

所以不同的 TXT 文件有不同的分隔符。默认分隔符是逗号。为了定义要使用的分隔符，我们在函数中使用了`sep`参数。在我们的例子中，我们使用:

```
import os
import pandas as pd# Find the full location of the working directory
LOCATION = "C:/Users/caleb/Desktop/PythonClasses/Class 3"# Change working directory
os.chdir(LOCATION)# Find the current working directory
training_data =  pd.read_csv("train.csv", sep = ",")
```

## 成块读取大型 CSV 文件

一个有趣的参数是`chunksize`,它允许您成块读取 csv 文件。我们可以要求熊猫逐行读取 50 个、100 个甚至更多，以优化文件的上传。

所以我们的代码应该是这样的:

```
import os
import pandas as pd# Find the full location of the working directory
LOCATION = "C:/Users/caleb/Desktop/PythonClasses/Class 3"# Change working directory
os.chdir(LOCATION)# Find the current working directory
training_data =  pd.read_csv("train.csv", 
                              sep = ",",
                              chunksize = 100)
```

## 将特定列作为数字或字符串读取

因此，如果我们想上传一个特定的列作为字符串或数字，我们必须使用`dtype`和一个字典，在这里我们定义列和我们想要使用的类型。

所以在我们的例子中，我们希望数据中的`***Survived***`列为真或假(布尔值)。为此，我们使用`dtpye`参数。

```
import os
import pandas as pd# Find the full location of the working directory
LOCATION = "C:/Users/caleb/Desktop/PythonClasses/Class 3"# Change working directory
os.chdir(LOCATION)# Find the current working directory
training_data =  pd.read_csv("train.csv", 
                              sep = ",",
                              chunksize = 100,
                              dtype = {'Survived': bool})
```

## 定义标题列

我们可以定义想要在 Python 中使用的列名。

我们甚至可以决定导入没有列名的文件，在这种情况下，我们将`header`设置为`None`，但是如果我们想要使用特定的行，我们将从`0`开始设置行的索引。

```
import os
import pandas as pd# Find the full location of the working directory
LOCATION = "C:/Users/caleb/Desktop/PythonClasses/Class 3"# Change working directory
os.chdir(LOCATION)# Find the current working directory
training_data =  pd.read_csv("train.csv", 
                              sep = ",",
                              chunksize = 100,
                              dtype = {'Survived': bool},
                              header = 0)
```

# 读取 Excel (XLSX)文件

我们对 CSV 文件所做的一切，都可以在 pandas 中使用`read_excel`函数对 XLSX 文件进行处理。因此，如果我们在同一位置有 train.xlsx，我们使用:

```
import os
import pandas as pd# Find the full location of the working directory
LOCATION = "C:/Users/caleb/Desktop/PythonClasses/Class 3"# Change working directory
os.chdir(LOCATION)# Find the current working directory
training_data =  pd.read_excel("train.xlsx",
                            dtype = {'Survived': bool},
                            header = 0)
```

**注意:**我们不能将`chunksize`与`read_excel.`一起使用，最简单的方法是使用 CSV 或 TXT 文件。