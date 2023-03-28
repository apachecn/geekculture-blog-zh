# 开发容器和 CLI 工具——天作之合

> 原文：<https://medium.com/geekculture/building-development-containers-and-cli-tools-a-match-made-in-heaven-a7a0e0719e06?source=collection_archive---------14----------------------->

## 如何教程

## 第 1 部分:如何用 VS 代码和 Docker 构建一个开发容器(外加一个演示视频)

![](img/c724900732bbc0fb5034f099f8c3baea.png)

Butterfly Nebula by [NASA](http://www.nasa.gov/), [ESA](http://www.spacetelescope.org/), and the Hubble SM4 ERO Team

ev 容器和 CLI 工具在开源和商业项目中越来越受欢迎。最近，我为我参与的开源项目[北极星 SLO 云](https://github.com/SLOCloud/SLOC)构建了一个开发容器。之前，我报道了主要的[经验教训](https://stefannastic.medium.com/why-every-open-source-project-needs-a-development-container-b1f5180ad5aa)。在这个由两部分组成的系列中，我将给出一个实践教程，介绍如何用 VS 代码和 Docker 构建开发容器(第 1 部分——本文)，以及如何用 Nx 创建 CLI 工具(第 2 部分)。

# 用 Docker 和 VS 代码构建一个开发容器

在这一节中，我将展示如何使用 VS 代码和 Docker 从头开始构建一个 dev 容器。在本文的后面，我将展示一个演示视频，演示如何使用 Visual Studio Code Remote-Containers 扩展来运行 dev 容器。为了能够跟随并运行演示，您首先需要安装:

*   [Docker Desktop for Windows/Mac](https://www.docker.com/products/docker-desktop)或 [Linux](https://docs.docker.com/get-docker/)
*   [Visual Studio 代码](https://code.visualstudio.com/)和[远程开发扩展包](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
*   [Minikube](https://minikube.sigs.k8s.io/) (可选；仅需要运行演示)

## 开发环境、源代码和开发容器

在本节的最后，我们将能够将运行在主机(例如笔记本电脑)上的 IDE(在本例中是 VS 代码)连接到我们的开发容器。这个容器将运行一个完全配置好的开发环境。最后，我们将能够从我们的 Git repo 中获取最新版本的源代码，并开始在我们的主机上运行的 ide 中进行开发。

源代码可以克隆在本地文件系统上，也可以克隆在*持久 docker 卷上。*在下面的演示中，我们将展示如何实现后者，因为由于[性能问题，目前不建议从本地文件系统挂载。](https://stefannastic.medium.com/why-every-open-source-project-needs-a-development-container-b1f5180ad5aa)有趣的是，容器和源代码之间没有依赖关系。这有利于共享开发容器，或者为开发人员提供额外的灵活性。

*完全配置的开发环境*应该包含所有必需的开发/构建工具，如语言 SDK 和 Git，运行时服务，如开发数据库，完整的配置模型，如网络和环境变量，以及完全配置的 IDE，以及所有必需的扩展、代码风格等等。现在让我们看看如何构建一个 dev 容器。

两个主要部分组成了一个开发容器:一个 *Dockerfile* 和一个 *devcontainer.json* 文件。实际上，通常会有第三个部分——一组自动化任务的 shell 脚本，例如配置文件管理和处理。为了使存储库“ *dev containers 准备好*”，需要将所有这些文件放在一个*中。存储库根目录中的 devcontainer* 目录。当存储库是 monorepo 时，这种方法存在一些问题，但这超出了本文的范围。

## 从 docker 文件构建开发容器

*Dockerfile* 只是一个普通的 Dockerfile，用于为 dev 容器构建容器映像。使用 VS 代码的一个好结果是微软提供了许多有用的[容器基础映像](https://github.com/microsoft/vscode-dev-containers/tree/master/containers)。

Dockerfile of our dev container (partial)

在我们的 *Dockerfile* 中，我们使用 typescript-node 作为我们的基本映像，它引入了许多有用的工具，比如:typescript、Node.js、Git 等等。我们将 *kubectl* 添加到容器中，因为这是运行我们定制的 Kubernetes 操作符所必需的。可以在本地运行整个堆栈，也包括 Kubernetes 集群。目前，我们只支持 Minikube 上的本地集群，它需要在主机上安装和运行。最后，使用 *Dockerfile* ，我们还向容器添加了一个定制的 bash 脚本。该脚本用于挂载主机的*。kubeconfig* 到容器中，安装网络并配置 Kubernetes 集群证书。为了保持主机和容器同步，脚本在每次登录时执行，例如，当 dev 容器从 IDE 启动时。

## 用 devcontainer.json 配置开发容器

dev 容器的第二个关键要素是一个 *devcontainer.json* 文件，它是特定于 VS 代码的。它基本上是一个配置文件，告诉 VS 代码如何构建和启动一个 dev 容器。它还提供了一些额外的开发环境配置。下面是我们的 *devcontainer.json* 配置的一个片段。

Devcontainer.json of our dev container (partial)

我们可以看到 *devcontainer.json* 支持配置一些有趣的东西，比如:

*   通过使用 *remoteEnv* 对象为 dev 容器声明环境变量，这比使用 Dockerfile 要干净一些。在我们的示例中，我们展示了一个简单的 env 变量*SYNC _ LOCALHOST _ kube config*，它由我们的 *copy-kube-config* bash 脚本使用。
*   通过简单地声明一个*挂载列表*，将目录从主机文件系统挂载到容器文件系统。这里我们展示了如何挂载一个本地。 *kube* 目录，包含本地(主机)Kubernetes 集群的所有配置文件。这使得我们的 Kubernetes 操作符(运行在我们的 dev 容器中)能够开箱即用地与本地集群进行交互，而无需任何额外的配置。
*   创建容器后，在容器内安装所需的 VS 代码*扩展*。这实现了统一的开发人员体验，并可用于实施一些公司/组织范围的策略。例如，代码格式化程序和 linters。
*   容器内的转发端口( *forwardPorts* )，以便它们在本地可用。容器启动后，VS 代码将在 ports 选项卡下显示本地地址，例如 *localhost:1234* 。这个特性在很多情况下非常有用，比如访问运行在容器中的服务或者在浏览器中查看 WebUI。我发现这比使用原生 Docker 支持更方便，例如，使用原生 Docker 支持需要知道在 Docker 中公开和发布端口之间的区别。
*   公开不同的生命周期挂钩，允许在容器中运行额外的命令。最有用的钩子是*postCreateCommand——它支持*安装额外的语言、框架或系统包/依赖项。它在装入源代码后触发。

这里有 *devcontainer.json* 选项的完整列表[。](https://code.visualstudio.com/docs/remote/devcontainerjson-reference)

# 演示视频

下面的 2 分钟演示展示了我们的 dev 容器的运行——从 *git 克隆*到在本地 Kubernetes 集群上运行定制控制器只需几分钟。

Development Containers Demo

# 结束语

在本文中，我们看到了如何构建一个 dev 容器。对于本教程，我使用了 Docker 和 VS 代码来构建 dev 容器，但是其他一些框架和工具也可以用于此目的。例如，一个有趣的替代品可能是 CNCF 的 [CNAB](https://cnab.io/) ，它带有一个独立的 CLI 工具。

一般来说，如果您有一些容器方面的经验，那么从 dev 容器开始并不奇怪。正如我们所看到的，它归结为声明一个 *Dockerfile* (也有许多很棒的预烘焙容器映像)，配置一个 *devcontainer.json，*，并可能添加一些定制脚本，例如，如果需要的话配置网络。就是这样！现在，任何人都可以在几分钟内开始为我们的项目做出贡献。

在第 2 部分中，我将展示如何构建一个 CLI 工具，并讨论为什么以及如何将它与开发容器结合起来。

你可以在我之前的文章中读到更多关于开发容器的好处和挑战:*[*为什么每个开源项目都需要一个开发容器？*](https://stefannastic.medium.com/why-every-open-source-project-needs-a-development-container-b1f5180ad5aa)*。**

**[](https://stefannastic.medium.com/why-every-open-source-project-needs-a-development-container-b1f5180ad5aa) [## 为什么每个开源项目都需要一个开发容器？

### 以及为什么你也应该建立一个…

stefannastic.medium.com](https://stefannastic.medium.com/why-every-open-source-project-needs-a-development-container-b1f5180ad5aa) 

如果你想了解更多关于我们的北极星 SLO 云项目，[查看我们的 GitHub](https://github.com/SLOCloud/SLOC) 或者关注我的更新。**