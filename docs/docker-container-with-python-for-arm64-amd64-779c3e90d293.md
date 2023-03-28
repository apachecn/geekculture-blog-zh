# 面向 ARM64/AMD64 的 Python docker 容器

> 原文：<https://medium.com/geekculture/docker-container-with-python-for-arm64-amd64-779c3e90d293?source=collection_archive---------7----------------------->

> ***更新:28.08.2022
> 注:*** *这是 Python 3.8，对于 Python 3.9 版本去这里*[*https://Alex-ber . medium . com/docker-container-with-Python-3-9-for-arm 64-amd64-f2cdf 167230 f*](https://alex-ber.medium.com/docker-container-with-python-3-9-for-arm64-amd64-f2cdf167230f)***更新结束***

本月，Anaconda 2021.05 发布。它首次包含对[“64 位 AWS Graviton2 (ARM64)平台”](https://docs.anaconda.com/anaconda/reference/release-notes/)的支持。你为什么在乎？让我们先来看看 ARM64 是什么的简短描述。

> ARM(高级 RISC 机，原为 Acorn RISC 机。RISC = Reduced Instruction Set Computer)是一款小巧节能，而非高性能的芯片组。因此，芯片组主要用于移动和节能设备，如智能手机、平板电脑、物联网等
> 
> x64 型通常用于更高性能的设备，如台式电脑、更高性能的笔记本电脑，甚至服务器和其他商用设备。

[https://www.quora.com/What-is-arm64-vs-x64](https://www.quora.com/What-is-arm64-vs-x64)

好吧，但是粗略地说，你为什么会选择性能较低的 CPU 呢？首先，你的代码可能会在 Raspberry pi 或 smartphone 上运行。然而，更有趣的情况是，以**的顺序支付更少的**。

如果你的代码无论如何都要在 AWS 中运行，那么选择 ARM64，你就可以*显著减少你的支付账单*。当然，您不能为任何应用程序这样做，但是有一些应用程序类您可以这样做。让我们看看 AWS 官方页面:

> Amazon EC2 A1 实例为扩展和基于 Arm 的应用程序提供了显著的成本节约，例如由广泛的 Arm 生态系统支持的 **web 服务器**、容器化**微服务**、缓存车队和分布式数据存储…大多数可以在 Arm 内核上运行的架构无关的应用程序也可以从 A1 实例中受益。

[https://aws.amazon.com/ec2/instance-types/a1/](https://aws.amazon.com/ec2/instance-types/a1/)

作为旁注，这意味着如果您正在使用神经元网络，使用 ARM64 可能是错误的想法。另一方面，如果您正在使用 Pandas 进行一些后台处理(例如 ETL)，这可能是可以的。但是如果你有网络服务/微服务，这是一个正确的选择。

> “如果您想使用 ARM 目标来减少您的费用，如 Raspberry Pis 和 AWS A1 实例，或者甚至继续使用您的旧 i386 服务器，到处部署可能会成为一个棘手的问题，因为您需要为这些平台构建您的软件”。

[https://www . docker . com/blog/multi-arch-build-and-images-the-simple-way/](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/)

## 重要提示:

只有`--*platform linux/arm64 alexberkovich/alpine-python3:latest*` 对 AWS A1 实例有效。

## 重要提示:

`Slim` `arm64` 的版本是测试版，可能会被删除，恕不另行通知。

你可以在下面的附录中了解我的[alpine-anaconda 3](https://github.com/alex-ber/alpine-anaconda3)s 项目的历史。

# 装置

源代码是我的[alpine-anaconda 3](https://github.com/alex-ber/alpine-anaconda3)项目的一部分。

您可以从 Dockerhub 通过 Python 安装 Docker 容器:

`docker pull alexberkovich/alpine-anaconda3`

或者对于 AMD64/Intel x86-x64:

`docker pull --platform linux/amd64 alexberkovich/alpine-anaconda3`

或用于 ARM/64/v8

`docker pull --platform --platform linux/arm64 alexberkovich/alpine-anaconda3`

您可以扩展这个 docker 图像，只需添加

```
FROM alexberkovich/alpine-anaconda3:latestCOPY conf/requirements.txt etc/requirements.txtRUN pip install -r etc/requirements.txt
```

> 注意:*这个 Docker 文件还有* slim *和* python *版本，几乎不包含 python 包。对于* python *版本，将 anaconda3 改为 python3。对于* slim *版本* *只需在 Docker 图片名称后追加* `*-slim*` *即可。请参阅下面的附录了解更多信息。*

# 概观

所以，我有了在 AMD64 上运行 Python 3.8.5 的[“基础 Python Docker 镜像”](https://github.com/alex-ber/alpine-anaconda3)。我想为 AMD64 和 ARM64 创建多拱门码头形象。

我真正想要的是，从用户的角度来看，我可以拥有可以在 AMD64 **和 arm 64**上工作的相同 Docker 映像。

实现这一点的工具是[多拱构建和图像](https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/)。实际上，我使用的是被描述为“与码头工人清单的艰难方式”。我发现一个可以接受的想法，我的构建应该直接部署到 Dockerhub。例如，为了创建瘦版本，我想积极地删除所有中间容器。使用`buildx`工具是不可能的。

# 在后台

我已经从现有的码头工人形象开始。它适用于 AMD64。所以，我想做的第一件事，是创建它的另一个版本，它适用于 ARM64，但带有最新版本的 Anaconda。

因为我的 Docker 镜像是基于 Alpine Linux 的，所以我不得不更新一些版本的二手 OS 级工具，比如`curl`。

然后我发现我需要添加 OS 级的包 *hdf5-dev。*在 AMD64 上，当我 *pip 安装 h5py* 时，它实际上下载并安装了轮子。对于 ARM64 来说，没有轮子可用，所以它从源代码安装，这要求 *hdf5-dev* 安装在操作系统级别。

一般来说，许多 Python 包没有用于 AMD64 的轮子，所以它们将从源代码安装。很可能，我已经做好了实现这一目标的一切准备(GCC 等)。

可能会让你吃惊，但是 AMD64 的 Anaconda 2021.05 和 AMR64 的 Anaconda 2021.05 包含的包并不完全相同。正如我以前说过的，解决这个问题的方法是从源代码安装。

然而，有一个重要的例外。[包](https://software.intel.com/en-us/blogs/2018/10/18/mkl-service-package-controlling-mkl-behavior-through-python-interfaces)(源代码[https://github.com/IntelPython/mkl-service](https://github.com/IntelPython/mkl-service))。它仅通过 Anaconda 提供，并且仅适用于 AMD64。对于 AMR64 可以用`mkl`代替`openblas`。还有一些英特尔封装不适用于 AMD64，但有替代产品。

在源代码级别，我创建了一个 Docker 文件，并从中创建了两个独立的 Docker 映像——一个用于 AMD64，一个用于 ARM64。我已经显示了这两个 Docker 图像集合，所以您可以编写`docker pull alexberkovich/alpine-anaconda3:latest`(这实际上是 manifest；它故意看起来像标签)并且 Docker 将拉出 Docker 图像的支持版本之一。你可以通过在 docker `pull` / docker `run`命令中提供`--platform=linux/amd64` 或`--platform=linux/amr64v8`，或者甚至在 docker 文件的请求中提供`--platform=linux/amd64` 或【】，来显式地控制它，就像这样:

Dockerfile 本身有几个 if 语句，有时我会根据是为 AMD64 还是 ARM64 构建而做不同的事情。比如`curl`就有不同的版本。在 Alpine Linux 中，我只能使用最新版本的 OS 级别的包，它们对于 AMD64 和 ARM64 是不同的。

在 Docker Hub 中，实际上有 3+3+3(+3)个不同的实体。

`alexberkovich/alpine-anaconda3:latest`是在撰写本故事时聚合的清单文件*alexberkovich/alpine-anaconda 3:0 . 3 . 3-amd64*和*alexberkovich/alpine-anaconda 3:0 . 3 . 3-arm 64v 8。*最后两个是常规标记图像(为特定 CPU 架构构建)。

`alexberkovich/alpine-anaconda3:latest-**slim**` 是本故事写作时聚合的清单文件*alexberkovich/alpine-anaconda 3:0 . 3 . 3-****slim****-amd64*和*alexberkovich/alpine-anaconda 3:0 . 3-****slim****-arm 64v 8。*最后两个是`*slim*`标记图像(为特定 CPU 架构构建)。

`alexberkovich/alpine-python3:latest`是在撰写本故事时聚合的清单文件*alexberkovich/alpine-python 3:0 . 3 . 3-amd64*和*alexberkovich/alpine-python 3:0 . 3 . 3-arm 64v 8。*最后两个是基于 Python(没有 Anaconda)的标记图像(为特定的 CPU 架构构建)。

`alexberkovich/alpine-anaconda3:latest`与`alexberkovich/alpine-anaconda3:0.3.3`相同(新版本发布后会有变化)。

`alexberkovich/alpine-anaconda3:latest-slim`与`alexberkovich/alpine-anaconda3:0.3.3-slim`相同(新版本发布后会有变化)。

`alexberkovich/alpine-python3:latest`与`alexberkovich/alpine-python3:0.3.3`相同(新版本发布后会有变化)。

# 附录

# 历史

大约 1.5 年前，我创建了我称之为[“基础 Python Docker 映像”](https://github.com/alex-ber/alpine-anaconda3)。它基于 Python 3.7 和 Anaconda。我创建它的原因是，我发现让*requirements . txt*并试图“只是” *pip 安装*对任何现有的 Docker 映像都不起作用。

如果我想安装一些全新的软件包，突然它无法工作，因为另一个依赖是旧的。

有时，我想删除一些软件包(因为它阻止我安装另一个；而且我不需要这个具体的)，但是我不能(可能需要卸载更多的包也可能是 distutil 项目，那个 pip 就是卸载不了，比如 *ruamel_yaml* 。

有时安装最新的新包会更新一半的现有依赖项，例如 *graphviz。*

另一个问题可能是，如果我有包 A 和包 B，我想安装这两个包，并且它们依赖于 *cffi* 包(Python 调用 C 代码的外部函数接口——许多具有 C 扩展的包都使用它),如果我运行命令 *pip install B* 和 *pip install A* 一切正常，但是如果我将它们放在*requirements . txt*中，这突然不起作用了。这是可能发生的，如果我把它们 A，B——按这个顺序。如果我已经在 Docker 中预装了 *cffi* ，那么顺序并不重要。

它也有一些操作系统级别的软件包，以便 pip 安装任何软件包“只是工作”。这意味着我安装了一些 SSL 相关的包，比如 *openssl-dev* 。为了能够成功地从源代码构建几乎任何包，我有几个 C/C++编译器，甚至是 Fortran 编译器。

在最新版本中，我添加了 *hdf5-dev* 操作系统级包。看来 *h5py (* 从 Python 中读写 HDF5 文件)要求它从源代码构建。

在以前的版本中，我已经将 Python 版本更改为 3.8。有一些包(比如熊猫/sklearn)我完全保留了它们的版本。

在最新版本中，我已经将 Anaconda 版本更新到了最新，所以大多数包都是最新的。尽管如此，还是有一些带 pinned 版本的包(对 pandas/sklearn 依然如此，但还有更多，例如 *h5py* 和 pyyaml(还有更多)。做出这样的决定有各种各样的原因，但是如果它不符合您的需要，您应该能够轻松地将它们的版本更新到您需要的版本。只是你应该知道，你可能还需要更新其他依赖关系。在安装了所有基础设施的帮助下，一旦你弄清楚了你想要更新的包的版本，你应该能够完成简单的 pip 安装。

# 超薄版

我创建 Docker 图像的最初动机是为了开发目的。它不是用来创建 Docker 容器来实际运行代码的。所以，Docker 图像的大小并没有困扰我。

如果我正在创建一些简单的微服务/web 服务，我不需要在其中包含 graphviz。实际上，大多数已安装的软件包都没有使用。

因此，我创建了我的[“基本 Python Docker 映像”](https://github.com/alex-ber/alpine-anaconda3)的“精简”版本，其中几乎不包含 Python 包。

我使用常规基础版本来创建*requirements . txt*——这个特定应用程序所需的所有包。然后我使用`alexberkovich/alpine-anaconda3-slim` 作为基础版本(从)在生产中实际运行它。

如果我需要添加新的依赖项，我会在常规的[“基础 Python Docker 映像”](https://github.com/alex-ber/alpine-anaconda3)上进行，更新 *requirements.txt* 并将基础映像更改为`alexberkovich/alpine-anaconda3-slim`。

您可以从 Dockerhub 安装带有 Python 的 slim Docker 容器:

`docker pull alexberkovich/alpine-anaconda3-slim`

或者对于 AMD64/Intel x86-x64:

`docker pull --platform linux/amd64 alexberkovich/alpine-anaconda3-slim`

或用于 ARM/64/v8

`docker pull --platform --platform linux/arm64 alexberkovich/alpine-anaconda3-slim`

# Python 版本

今天我终于添加了 Python 版本。根本不包括蟒蛇。我正在使用 Alpine Linux 软件包管理安装`*python-dev*` OS 级软件包。然后我在添加一些包，让它接近`*slim*`版本。

它作为`*slim*` 版本的替代方案。注意，一些包，比如 *sip* 在这个版本中不可用。对于 AMD64 版本，像 *mkl_random* 这样的包不可用，使得 *numpy* 无法使用。对于 ARM64 *llvmlite，*例如*，*不可用，无法从源代码构建许多包。

您可以从 Dockerhub 安装带有普通 Python 的 Docker 容器:

`docker pull alexberkovich/alpine-python3`

或者对于 AMD64/Intel x86-x64:

`docker pull --platform linux/amd64 alexberkovich/alpine-python3`

或用于 ARM/64/v8

`docker pull --platform --platform linux/arm64 alexberkovich/alpine-python3`

**参见:**

[在 Docker 容器中使用 GNOME 钥匙圈](/analytics-vidhya/using-gnome-keyring-in-docker-container-2c8a56a894f7)