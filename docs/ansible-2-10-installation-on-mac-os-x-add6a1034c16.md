# 在 Mac OS X 上安装 Ansible 2.10

> 原文：<https://medium.com/geekculture/ansible-2-10-installation-on-mac-os-x-add6a1034c16?source=collection_archive---------1----------------------->

## 可行的安装可不那么简单…

## Ansible 2.10 就在那里。什么是 Ansible？怎么安装？

![](img/30144edd72d52403abbae2f7fddf1862.png)

Photo by [Alex Knight](https://unsplash.com/@agkdesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

我是一个可靠的用户已经有几年了。我在家里和工作中使用它来配置从计算机到网络设备的各种设备。或多或少，它通过 Ansible 管理我们所有的基础设施。

在本文中，我将给出我对 Ansible 关键概念的理解。对工具的概述来说重要的和重要的。

开始安装后，我们将发现一些工具。他们可以帮助你有一个友好的工作环境。

**‼️免责声明**:自从我最初写这篇文章以来，时间已经过去了。内容应该与安装部分不太相关。它仍然与关键概念相关😇。

# 什么是 Ansible？

RedHat 表示:Ansible 是一款无代理解决方案，可实现 IT 管理自动化。

> 任何人都可以使用的简单、无代理的 IT 自动化。Ansible 是一种通用语言，它揭开了工作如何完成的神秘面纱。将艰巨的任务转化为可重复的行动手册。只需按一下按钮，即可推出企业范围的协议。— [RedHat Ansible 网站](https://www.ansible.com/)

![](img/4c17ce4e56614cfde0a437b5f0f2f12e.png)

Photo by [Maximalfocus](https://unsplash.com/@maximalfocus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我在工作和私人生活中使用 Ansible 已经有好几年了。我用它来管理我的家用电脑。我在工作中使用它来管理或多或少的一切，以将“[基础设施作为代码](https://en.wikipedia.org/wiki/Infrastructure_as_code)”。

那这个词本身呢？看到它与科幻世界有关是很有趣的。这个词是科幻发明。

> 一种**可能的**是一种能够进行近瞬时或[超光速通信的虚拟设备或技术](https://en.wikipedia.org/wiki/Faster-than-light_communication)。它可以跨越任何距离或障碍向相应的设备发送消息或从相应的设备接收消息，而没有延迟，甚至在星际系统之间也是如此。作为这种设备的名称，“ansible”一词首次出现在厄休拉·K·勒·古恩 1966 年的一本小说中。从那时起，这个术语被广泛用于众多科幻作家的作品中，跨越各种背景和连续性。— [维基百科](https://en.wikipedia.org/wiki/Ansible)

在接下来的几节中，我们将介绍 Ansible 的一些最重要的概念。我不想假装详尽或准确。这是我对这些概念的理解和务实态度。

## 剧本

行动手册允许您编写一套任务来配置和管理主机。从网络到应用程序配置，我们可以做或多或少的事情。而更重要的是幂等原则。

> **幂等性**是[数学](https://en.wikipedia.org/wiki/Mathematics)和[计算机科学](https://en.wikipedia.org/wiki/Computer_science)中某些[运算](https://en.wikipedia.org/wiki/Operation_(mathematics))的属性，由此它们可以被多次应用，而不改变最初应用之外的结果。幂等性的概念出现在[抽象代数](https://en.wikipedia.org/wiki/Abstract_algebra)(特别是在[投射器](https://en.wikipedia.org/wiki/Projector_(linear_algebra))和[闭包操作符](https://en.wikipedia.org/wiki/Closure_operator)的理论中)和[函数编程](https://en.wikipedia.org/wiki/Functional_programming)(其中它连接到[引用透明](https://en.wikipedia.org/wiki/Referential_transparency)的属性)的许多地方。— [维基百科](https://en.wikipedia.org/wiki/Idempotence)

![](img/e89669b2b75bfeeafe3852588eae46cb.png)

Photo by [Louis Smith](https://unsplash.com/@zion_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Ansible 中，幂等性描述了任何任务都可以运行两次或更多次的事实。它总是会产生相同的结果。对我来说，这是 Ansible 的核心概念之一，也是最需要理解的一个。在使用 Ansible 时，您总是依赖这个原则。大多数情况下，可转换模块实现幂等性。这将避免你必须照顾这一点。

Playbook Sample

## 角色和任务

Ansible 中的另一个概念是角色。角色是编写可重用任务的一种方式。然后，在行动手册中，您可以调用一组角色和任务来代替仅有的任务。它会让你有可能在不同的上下文中使用相同的角色。

Role Directory Structure

Ansible 中的任务是应用配置的基本元素。Ansible 中提供了许多不同的任务。例子有`copy`、`file`和`template`。这些核心任务操作文件。

Role Sample

角色是一个或多个连续任务的组合。

## 库存

另一个重要的概念是库存。清单是您将应用行动手册一个或多个主机的列表。您可以定义静态库存或建立动态库存。它是通过运行行动手册来配置的一组计算机。

Inventory Sample

## 组件

Ansible 中的模块是一组有组织的任务。例如，您将找到一个用于`AWS`、`Azure`和`filesystem`的模块。Ansible 或其社区支持大量的模块。我们可以说这或多或少是一种按主题组织 Python 代码的方式。

## 收藏品

Ansible 文档将集合描述为分发内容的一种方式。内容可以是角色、行动手册、模块或插件。

> 集合是可转换内容的分发格式，可转换内容包括行动手册、角色、模块和插件。随着模块从核心 Ansible 存储库转移到集合中，模块文档将转移到[集合页面](https://docs.ansible.com/ansible/latest/collections/index.html#list-of-collections)。— [可行文件](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html)

对我来说，这是封装 Ansible 的每一个不属于 Ansible 核心的额外内容的新方法。这是 Ansible 的一大进步，为该工具带来了更多的模块化。

# 可行的安装

但是在写我的收藏之前，我需要先安装 Ansible。在写这篇文章的时候，Ansible 2.10 还不能通过`homebrew`获得。发布日期是在八月，而`homebrew`的软件包可能需要一段时间才能上市。

要安装 Ansible，需要用`homebrew`运行这个命令。

```
brew install ansible
```

但是，从 2.10 版本开始，他们似乎也修改了内核。考虑我是在 Mac OS X 下运行的。

[](https://docs.ansible.com/ansible/devel/installation_guide/intro_installation.html#installing-ansible-on-macos) [## 安装 Ansible — Ansible 文档

### 本页描述如何在不同的平台上安装 Ansible。Ansible 是一个无代理的自动化工具，由…

docs.ansible.com](https://docs.ansible.com/ansible/devel/installation_guide/intro_installation.html#installing-ansible-on-macos) 

于是我用 Python 包管理器之一的`pip`安装了 Ansible。首先，我需要安装正确版本的 Python。我安装了 Python 3.8.5。为此，我使用了`asdf-vm` cli 工具。它管理各种版本的开发环境和语言。

 [## asdf——一个可扩展的版本管理器

### 可扩展的版本管理器

可扩展版本的 managerasdf-vm.com](https://asdf-vm.com) 

要安装 Python 3.8.5，请运行以下命令(粗线)。

```
*# Add the ASDF Python plugin to manage versions for this language*
❯ **asdf plugin-add python**updating plugin repository...
remote: Enumerating objects: 59, done.
remote: Counting objects: 100% (59/59), done.
remote: Total 97 (delta 59), reused 59 (delta 59), pack-reused 38
Unpacking objects: 100% (97/97), done.
From https://github.com/asdf-vm/asdf-plugins
   d736d70..e1bc152  master     -> origin/master
HEAD is now at e1bc152 Merge pull request #286 from abatilo/patch-2*# Install the required Python version*
❯ **asdf install python 3.8.5**python-build 3.8.5 /Users/<user>/.asdf/installs/python/3.8.5
python-build: use openssl@1.1 from homebrew
python-build: use readline from homebrew
Downloading Python-3.8.5.tar.xz...
-> https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tar.xz
Installing Python-3.8.5...
python-build: use readline from homebrew
python-build: use zlib from xcode sdk
Installed Python-3.8.5 to /Users/<user>/.asdf/installs/python/3.8.5*# Make Python version as the global default version*
❯ **asdf global python 3.8.5***# If everything works well, you should get the right version
# You may have to restart your terminal session*
❯ **python -V**Python 3.8.5*# I also upgrade pip when I install Python*
❯ **pip install --upgrade pip**Collecting pip
  Downloading pip-20.2.3-py2.py3-none-any.whl (1.5 MB)
     |████████████████████████████████| 1.5 MB 2.7 MB/s
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 20.1.1
    Uninstalling pip-20.1.1:
      Successfully uninstalled pip-20.1.1
Successfully installed pip-20.2.3
```

现在我们可以用下面的命令(粗线)安装 Ansible 2.10。

```
*# Install Ansible base (new since 2.10)*
❯ **pip install ansible-base**Processing ./Library/Caches/pip/wheels/55/b6/74/6bf7a932107b1e032a4d33c41433ebf56620ad1f3cc2068f8e/ansible_base-2.10.1-py3-none-any.whl
Collecting jinja2
  Using cached Jinja2-2.11.2-py2.py3-none-any.whl (125 kB)
Collecting packaging
  Using cached packaging-20.4-py2.py3-none-any.whl (37 kB)
Processing ./Library/Caches/pip/wheels/13/90/db/290ab3a34f2ef0b5a0f89235dc2d40fea83e77de84ed2dc05c/PyYAML-5.3.1-cp38-cp38-macosx_10_15_x86_64.whl
Collecting cryptography
  Using cached cryptography-3.1-cp35-abi3-macosx_10_10_x86_64.whl (1.8 MB)
Collecting MarkupSafe>=0.23
  Using cached MarkupSafe-1.1.1-cp38-cp38-macosx_10_9_x86_64.whl (16 kB)
Collecting pyparsing>=2.0.2
  Using cached pyparsing-2.4.7-py2.py3-none-any.whl (67 kB)
Collecting six
  Using cached six-1.15.0-py2.py3-none-any.whl (10 kB)
Collecting cffi!=1.11.3,>=1.8
  Using cached cffi-1.14.2-cp38-cp38-macosx_10_9_x86_64.whl (176 kB)
Collecting pycparser
  Using cached pycparser-2.20-py2.py3-none-any.whl (112 kB)
Installing collected packages: MarkupSafe, jinja2, pyparsing, six, packaging, PyYAML, pycparser, cffi, cryptography, ansible-base
Successfully installed MarkupSafe-1.1.1 PyYAML-5.3.1 ansible-base-2.10.1 cffi-1.14.2 cryptography-3.1 jinja2-2.11.2 packaging-20.4 pycparser-2.20 pyparsing-2.4.7 six-1.15.0*# Make sure Ansible is in your path*
❯ **asdf reshim python***# Check the installed version and correct Python version*
**❯ ansible --version**ansible 2.10.1
  config file = /Users/<user>/.ansible/ansible.cfg
  configured module search path = ['/Users/<user>/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /Users/<user>/.asdf/installs/python/3.8.5/lib/python3.8/site-packages/ansible
  executable location = /Users/<user>/.asdf/installs/python/3.8.5/bin/ansible
  python version = 3.8.5 (default, Sep 20 2020, 11:09:32) [Clang 11.0.3 (clang-1103.0.32.59)]
```

在这个阶段，您有一个 Ansible 的基础安装，其中只包含核心元素。测试行动手册和配置文件。它迫使我安装额外的 Ansible 集合。

My Ansible Configuration File

创建一个文件`test.yaml`来验证您的 Ansible 安装。该文件必须包含以下内容。

Test Playbook

剧本的运行产生了错误。该错误是由于我的 Ansible 配置文件中的以下配置造成的。

```
stdout_callback = yaml
bin_ansible_callbacks = True
```

以及错误。

```
*# The playbook command*
❯ **ansible-playbook test-playbook.yaml**...
ERROR! Invalid callback for stdout specified: yaml
```

为了解决这个问题，我必须安装`community.general`集合。

 [## 不稳定星系

### 从 Ansible 社区的精彩内容开始您的自动化项目

galaxy.ansible.com](https://galaxy.ansible.com/community/general) 

为了安装该集合，我执行了以下命令。我得到一个错误，因为`collections`文件夹不存在。

```
*# Tried to list the installed collections*
❯ **ansible-galaxy collection list**[WARNING]: — the configured path /Users/<user>/.ansible/collections does not exist.
....
ERROR! — None of the provided paths were usable. Please specify a valid path with — collections-path
```

我创建了文件夹，然后安装了收藏。

```
*# Create the directory*
❯ **mkdir /Users/<user>/.ansible/collections***# List installed collections*
❯ **ansible-galaxy collection list***# None installed, empty stdout**# Install the collection*
❯ **ansible-galaxy collection install community.general**Starting galaxy collection install process
Process install dependency map
Starting collection install process
Installing 'community.general:1.1.0' to '/Users/<user>/.ansible/collections/ansible_collections/community/general'
Downloading [https://galaxy.ansible.com/download/community-general-1.1.0.tar.gz](https://galaxy.ansible.com/download/community-general-1.1.0.tar.gz) to /Users/<user>/.ansible/tmp/ansible-local-90875hlzn08qq/tmpwt8d2kpe
community.general (1.1.0) was installed successfully
...
google.cloud (1.0.0) was installed successfully
...
ansible.posix (1.1.1) was installed successfully
...
ansible.netcommon (1.2.1) was installed successfully
...
community.kubernetes (1.0.0) was installed successfully
```

最后，我可以运行测试手册了。

```
*# Run the test playbook*
❯ **ansible-playbook test-playbook.yaml**PLAY [localhost] ********************************************************************TASK [debug] ********************************************************************
ok: [localhost] =>
  msg: Hello World!PLAY RECAP ********************************************************************
localhost           : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

# 结论

我们有一个可行的设置，可以运行剧本和其他可行的命令。从这一点，我们可以开始自动化我们想要的一切(如果模块和插件存在)。

我们也准备深入研究 Ansible，自己写一些模块。在以后的文章中，我将介绍如何为 Ansible 编写一个模块。

# 参考

Ansible 官方网站包含许多资源。它还包含模块的所有文档。此外，还提供了安西布尔塔(商业产品)和 AWX 的链接。

[](https://www.ansible.com) [## Ansible 是简单的 IT 自动化

### Ansible 是自动化应用和 IT 基础设施的最简单方式。应用程序部署+配置管理+…

www.ansible.com](https://www.ansible.com) 

家酿是 Mac OS X 上的首选打包管理器。它运行良好，社区也很活跃。

[](https://brew.sh/) [## 公司自产自用

### macOS(或 Linux)缺失的软件包管理器。

brew.sh](https://brew.sh/) 

您的编程语言和其他 cli 工具的终极版本管理器。

 [## asdf——一个可扩展的版本管理器

### 可扩展的版本管理器

可扩展版本的 managerasdf-vm.com](https://asdf-vm.com/)