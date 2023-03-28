# 诀窍:简单的需求构建

> 原文：<https://medium.com/geekculture/trick-easy-requirements-build-b4e00e2439be?source=collection_archive---------1----------------------->

## 如何在不到 2 分钟的时间内为您的项目提出需求——无论是小型项目还是大型项目。

![](img/ca86fd43c9c154eed69a21c73bfe1146.png)

Photo by [Dlanor S](https://unsplash.com/@dlanor_s?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 摘要

*   语境；
*   安装和使用；
*   观察；
*   资源。

# 语境

需求文件用于存储项目中使用的库和包，或者如 [pip 文档](https://pip.pypa.io/en/stable/user_guide/#requirements-files)中所述:

> “需求文件”是包含要使用 [pip install](https://pip.pypa.io/en/stable/cli/pip_install/#pip-install) […]安装的项目列表的文件

因此，在 Python 项目中找到这些名为 requirements.txt 的需求文件是非常常见的。这些需求的安装可以很容易地使用如下命令来执行，或者更具体地说，使用一些也可以在 [pip 文档中找到的参数。](https://pip.pypa.io/en/stable/cli/pip_install/#)

```
$ pip install -r requirements.txt --user
```

但是，几天前我用 Python 做了一个项目，想让任何想访问它的人都可以实践一下，其中一个步骤是构建 requirements.txt，它通常加载必要的包来运行 Python 中的项目，最简单的替代方法是:

*   逐个查找并选择项目中存在的包；
*   在您的终端中运行以下命令，并选择所需的包及其版本。

```
$ pip freeze
```

但是这**一点都不实用**，更不用说**可扩展**了，因为根据项目的大小，这可能需要**大量时间**，并且你冒着**忘记一些 package**e 并遇到错误的风险，或者项目的用户会因为这种缺乏而遇到问题。

因此，我寻找一个能满足我需求的替代方案，并找到了一个能做到这一点的项目。**它的目标是在不到两分钟的时间内，基于项目中的导入自动生成 requirements.txt 文件。**

# 安装和使用

要安装这个软件包，只需运行:

```
$ pip install pipreqs --user
```

或者，如果您使用 Python3:

```
$ pip3 install pipreqs --user
```

要自动构建 requirements.txt，只需在项目目录中运行以下命令:

```
$ pipreqs .
```

或者

```
$ pipreqs /project/location
```

奇迹将会发生！！

![](img/a97b5afed9775297434ea44f7d2e9b8b.png)

Photo by [Pierrick VAN-TROOST](https://unsplash.com/@vantroostpierrick?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 观察

如果您已经有一个 requirements.txt，那么您将需要强制生成新的需求，因此运行以下命令:

```
$ pipreqs . --force
```

希望这篇帖子有帮助！；)
感谢阅读！

# 资源

这篇文章的灵感来自于下面的知识库:

[](https://github.com/bndr/pipreqs) [## bndr/pipreqs

### 基于任何项目的导入生成 pip requirements.txt 文件。寻找维护者来移动这个…

github.com](https://github.com/bndr/pipreqs)  [## pip 安装- pip 文档 v21.1.1

### 使用需求说明符从:PyPI(和其他索引)安装软件包。VCS 项目网址。本地项目…

pip.pypa.io](https://pip.pypa.io/en/stable/cli/pip_install/#requirements-file-format)