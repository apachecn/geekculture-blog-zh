# 缩小 JS 文件

> 原文：<https://medium.com/geekculture/minifiying-js-files-861cdd7b214c?source=collection_archive---------10----------------------->

离线选择

![](img/a9bb950dd0ca398098fcc1e1c93df042.png)

Photo by [Fahrul Razi](https://unsplash.com/@mfrazi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

让我们的网站加载更快的方法之一是缩小我们的 Javascript 文件。还有一个额外的好处:缩小 Javascript 文件使得外行人更难读懂，因此为*提供了一些针对抄袭者的*保护(只有缩小才能被逆向工程，我们需要一个混淆器来代替。这篇文章并没有谈到这一点，但是一个混淆器的例子可以在这里找到。

# 在线微型打印机

这只是可用工具的一小部分，但这些是我最常用的工具:

1.  [谷歌闭包编译器](https://closure-compiler.appspot.com/home)
2.  [JSCompress](https://jscompress.com)
3.  [缩小](https://www.minifier.org)

# 离线缩小器

有时我变得偏执，我宁愿使用不在服务器上的东西，而不是把东西粘贴到别人的页面上。并且 [RequireJS](https://requirejs.org) 有一个我们可以使用的优化器。

**第 0 步:获取需求**

我在节点中使用 [RequireJS，并在 Docker 上运行 NodeJS。我没有使用 npm 安装 requirejs，因为对于我的用例来说有一个更简单的方法——只需](https://requirejs.org/docs/node.html)[获取 r.js 文件](https://requirejs.org/docs/release/2.3.6/r.js)，如这里的[所列](https://requirejs.org/docs/download.html#rjs)，在最新版本下。

按照惯例，我假设我们要缩小的文件的名称是 myfile.js。

第一步:准备文件

创建一个文件夹，例如~Documents/MyFolder，并将 r.js 和 myfile.js 放入该文件夹。

**第二步:在 Docker 上运行节点**

假设我们已经获取了最新的 Docker 映像，在 Docker 桌面下的节点映像上单击 Run。打开可选设置。在“卷”下，将主机路径设置为～Documents/my folder(我们在步骤 1 中创建的文件夹)。将容器路径设置为/home。单击运行。

**第三步:输入命令**

容器运行时，单击 CLI 图标。这将打开一个新的终端窗口(我在 Mac 上)。

导航到/home 文件夹。

```
cd home
```

通过使用以下命令，我们应该可以在该文件夹中看到我们的文件 r.js 和 myfile.js:

```
ls
```

然后，我们可以使用以下方法缩小文件:

```
node r.js -o name=myfile out=myfile.min.js baseUrl=.
```

产生的缩小文件与闭包编译器生成的文件相当。

感谢您阅读本文！我希望这在某种程度上对你有用。当我在开发中遇到一些东西时，我会写一些片段，以一种解释的方式。如果你有兴趣阅读更多，请查看我的其他文章或在 Twitter 上关注我。