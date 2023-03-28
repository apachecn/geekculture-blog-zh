# 准备好了吗？设置。Python！

> 原文：<https://medium.com/geekculture/ready-set-python-78a9272dda71?source=collection_archive---------70----------------------->

Python 是一种非常强大的语言。在我过去的一些文章中，我讨论了如何使用 python 创建自己的语言，以及如何利用 for 循环的更多功能。在本文中，我将介绍 Python 的其他强大特性。

## 1.您可以创建一个 for 循环，而不实际使用 for 循环！

在 python 中，可以使用`exec`函数和字符串乘法来创建一个 for 循环，如下所示:

## 2.你可以使用 Python 发送电子邮件

这个有点复杂。你需要设置一个 gmail 帐户，并为该帐户打开[不太安全的应用](https://myaccount.google.com/lesssecureapps)。

然后，您需要使用 python 库中的`smtplib`和`email`编写一个 python 脚本。

发送电子邮件有许多不同的方式，其中最简单的是[这里是](https://stackoverflow.com/questions/6270782/how-to-send-an-email-with-python)，但我个人最喜欢的是下面的方式:

## 3.您可以将 python 文件转换成 exe 文件

长期的 python 程序员可能知道 Python 运行在 C++上。这就是为什么 Python 没有提供编译成 exe 的内置方式可能会让人感到惊讶。

然而，有很多解决方法。

打开新的终端或 cmd 窗口，并键入以下命令:

```
pip install cx_Freez
pip install idna
```

这两个库是转换所需要的。将文件复制到。exe 文件。接下来，创建 python 脚本，如下所示:

就是这样！

## 4.您可以在没有 CMD 窗口的情况下运行 Python

如果您对将 python 文件转换成 exe 不感兴趣，您可以简单地更改文件扩展名。

如果你的文件被命名为`name.py`，将其重命名为`name.pyw`。现在，双击该文件，它将在没有窗口的情况下运行。这种类型的文件称为“Python 文件(无控制台)”。

## 5.您可以使用 Python 运行命令

使用 python，可以直接写入命令行。因此，您可以从 python 内部调用其他 python 文件:

这就是本文的全部内容。我希望你学到了新的东西！t