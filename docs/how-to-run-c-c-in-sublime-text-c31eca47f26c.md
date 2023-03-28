# 如何在 Sublime 文本中运行 C/C++？

> 原文：<https://medium.com/geekculture/how-to-run-c-c-in-sublime-text-c31eca47f26c?source=collection_archive---------4----------------------->

学习在 Sublime Text 中运行 C 或 CPP，竞赛编程的救星

![](img/917e9a2c5696d5ab7a2c78626255d91c.png)

[Christopher Gower](https://unsplash.com/@cgower) **—** [**Unsplash**](https://unsplash.com/)

C 和 C++是世界上最强大的编程语言。大部分超快超复杂的库和算法都是用 C 或 C++写的。大部分强大的内核程序也是用 c 写的，所以，没办法跳过。

大多数程序员在编程竞赛中更喜欢用 C 或 C++写代码。 [**游客**](https://en.wikipedia.org/wiki/Gennady_Korotkevich) 被认为是各个年龄段用 C++写代码的世界顶级编程选手。

在编程竞赛期间，程序员更喜欢使用轻量级编辑器来专注于编码和算法设计。 [Vim](https://www.vim.org/) 、 [Sublime Text](https://www.sublimetext.com/) 、 [Notepad++](https://notepad-plus-plus.org/) 是最常见的编辑器。除了竞争，许多软件开发人员和专业人士喜欢使用 Sublime Text，因为它的灵活性。

![](img/362312007dcc8688f1b9c860f166fb0e.png)

Gennady Korotkevich ( aka Tourist) — Belarusian Sport Programmer — Photo Wikipedia

我在这篇博文中讨论了在 Sublime 文本中运行 C/C++代码之前我们需要完成的步骤。我们将从一个**输入文件**获取输入，并将输出打印到一个输出文件的**中，而不使用 C/C++中的`freopen`文件相关函数。**

# 安装崇高的文本

*   对于 **Windows** ，可以从 [**链接**](https://download.sublimetext.com/sublime_text_build_4107_x64_setup.exe) 下载。
*   对于 **Ubuntu 或者 Debian** ，我们可以安装如下:

—安装 GPG 键:

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```

—现在，我们选择要使用的稳定通道:

```
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
```

—更新和安装 Sublime 文本:

```
sudo apt-get update
sudo apt-get install sublime-text
```

*   对于 **macOS** ，我们可以从这个 [**链接**](https://download.sublimetext.com/sublime_text_build_4107_mac.zip) 下载。

# JSON for Package 在 Sublime Text 中新构建的包

我们主要关注的是自动运行我们的代码，从文件中获取输入，而不是每次运行代码时手动输入。

因此，我们必须添加一个新的构建系统。我们必须进入`**Tools > Build System > New Build System**`，然后添加以下代码并保存文件:

最后，我们现在可以通过按下`Command + B`来构建我们的代码，并看到`in.txt`文件输入到`out.txt`文件的输出。

**注意:** `in.txt`，`out.txt`和源代码应该在同一个目录下。

[![](img/2093d0f16d94a8942508624035f676b1.png)](http://buymeacoffee.com/habibrahman)

如果需要循序渐进，可以看完整的视频教程。

# **结论**

感谢您宝贵的时间和阅读这篇文章。

如果你觉得这篇文章有帮助，请随意评论、分享和鼓掌👏你的手。更多这样的帖子请关注我 [**中**](/@habibrahmanbd) 。你可以访问我的 [**网页简介**](https://habibrahman.me/) ，阅读我的 [**博文**](https://blog.habibrahman.me/) ，关注我的 [**推特**](https://twitter.com/habib_rahman_bd) 。

你可以查看 [**这篇**](https://python.plainenglish.io/basic-web-scraping-and-html-parsing-using-beautifulsoup-bs4-python-library-799b9e41268f) 文章来学习网页抓取和 HTML 解析的基础知识。

**资源:**

1.  [https://blog . habibrahman . me/ide/2019/10/19/Running-C-c++-Codes-in-Sublime-Text/](https://blog.habibrahman.me/ide/2019/10/19/Running-C-C++-Codes-in-Sublime-Text/)
2.  [https://en.wikipedia.org/wiki/Gennady_Korotkevich](https://en.wikipedia.org/wiki/Gennady_Korotkevich)
3.  [https://www.sublimetext.com/](https://www.sublimetext.com/)