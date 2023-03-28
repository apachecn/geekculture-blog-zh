# Red Hat OpenShift 开发和部署企业 ML/DL 应用程序

> 原文：<https://medium.com/geekculture/red-hat-openshift-to-develop-and-deploy-enterprise-ml-dl-applications-ce7d75d808c4?source=collection_archive---------58----------------------->

![](img/b3943cadee516f47741d4fc1427560f7.png)

Photo by Annamária Borsos

对机器学习和深度学习(ML/DL)的投资正在显著增加，以通过不同的目标创造价值，如掩盖复杂性、自动化、降低成本、发展业务、更好地服务于客户、发现、研究和创新等。Kubernetes 和 Openshift 上强大的 ML/DL 开源社区已经创建并不断发展。这些社区致力于允许数据科学家和开发人员访问和使用 ML/DL 技术。在我们的本地计算机上工作并将 ML/DL 模型投入生产需要在一个巨大而复杂的空间中导航。我们在哪里部署用于训练的代码，在哪里部署用于批量或在线推理的代码。有些情况下，我们需要在多架构环境或混合云环境中部署我们的机器学习工作流。如今的数据中心由异构系统(x86、IBM Power Systems、IBM Z、高性能计算、GPU 或 FPGAs 等加速器……)组成，运行异构工作负载，采用专门的 ML/DL 框架，各有所长。除此之外，我们可以看到云的所有维度(公共云和私有云、混合云、多云、分布式云)。例如，我们可以在 IBM Power Systems 上运行一个包含关键数据的数据库，我们希望将其用于我们的模型，使用 GPU 运行我们的训练代码，在关键事务应用所在的 IBM Z / LinuxONE 上部署批处理或在线推理，并避免延迟，避免在云上或边缘进行另一次推理。根据您的业务，有许多重要的选项需要考虑。典型的 ML/DL 工作流始于业务目标，涉及理解用户、挑战假设、重新定义问题和共同创建(例如将 IT 和数据科学家团队放在同一个房间)原型和迭代测试解决方案的设计思维。然后，我们收集私人和公共数据，提炼和存储数据，创建和验证模型，直到我们将一切投入到现实世界的生产中。我们需要考虑应用程序的可伸缩性、弹性、版本、安全性、可用性等。这需要额外的专业知识，通常是专门的硬件资源，从而增加了对资源管理和利用的需求。数据科学家无法管理复杂的整个流程，就我个人而言，他们希望获得高性能的硬件，专注于数据和模型的创建。这也是为什么我们可以看到深度工作没有被完全开发，因为它没有为生产做好准备。这就是 containers 和 Kubernetes 可以通过加速 ML/DL 的采用来避免这种情况并打破所有这些障碍的地方。现在有一个明显的趋势，那就是使用 Linux 容器和 Kubernetes 来开发 ML/DL 应用程序，并部署它们。容器和 Kubernetes 通过屏蔽复杂性来简化对底层基础设施的访问，允许管理不同的工作流，如开发或应用程序生命周期。Red Hat OpenShift 将提供非常适合企业环境的附加功能

# 什么是 OpenShift？

Kubernetes 是一个开源项目，Red Hat OpenShift 是一个经过认证的 Kubernetes 平台和发行版。这是一个基于 Kubernetes 的容器应用程序平台，用于企业应用程序开发和部署。Red Hat 是 Kubernetes 社区的主要贡献者之一。OpenShift 是一系列容器化软件，如 OpenShift Online、OpenShift Dedicated 或 OpenShift Container Platform，后者是一种本地平台即服务，由 Kubernetes 在 Red Hat Enterprise Linux 上编排和管理 Docker 容器。OKD 是 OpenShift 的开源版本，在 2018 年 8 月之前被称为 OpenShift Origin (Origin 社区分发)。它是上游社区项目。OpenShift 背后的想法实际上是为了增强管理和开发人员在 Kubernetes 上部署企业(大规模)应用程序的体验。

# OpenShift 和 Kubernetes 有什么区别？

Kubernetes 有许多发行版，如果我们需要帮助，大部分依赖于社区或外部专家。公司通常会寻求官方支持，尤其是在你经营关键业务的情况下，但不仅限于此。为了完全运行 Kubernetes 环境并在分布式系统环境中运行容器化的应用程序，当您的目标是在 Kubernetes 上部署企业应用程序时，我们需要的不仅仅是 Kubernetes 的专业知识。我们需要考虑多种因素，例如强大的安全性、开发人员友好的环境、集群管理、集成构建和 CI/CD 服务、多架构和多平台部署(无论在数据中心的任何位置)、公共云、多云、边缘、虚拟机、裸机、x86、IBM Power Systems、IBM Z、部署映像的注册表、操作自动化、安全容器映像、管理和自动容器更新、管理混合存储、多租户和多集群管理等。安全性的话题确实是一个关键的元素，如果我们可以通过在默认情况下拥有一个更安全的基础来避免令人头痛的问题，这肯定会有所帮助。Red Hat OpenShift 上的默认策略比 Kubernetes 上的更严格。例如，基于角色的访问控制(RBAC)是 OpenShift 不可或缺的一部分。对于一个小的开发/测试设置，在没有 RBAC 安全的情况下使用 Kubernetes 可能没问题，但是当现实生活和生产到来时，有一定级别的权限是必要的。Kubernetes 就像一个 Linux 内核。我们需要的不仅仅是一个 Linux 内核，我们需要一个 Linux 平台发行版来运行 Linux 应用程序。在实践中，对于那些已经安装并使用 Kubernetes 的用户，Red Hat OpenShift 支持使用 **kubectl** ，用户可以使用原生的 Kubernetes 命令行界面。我们还可以为开发人员使用命令行，并利用其他命令行工具的额外功能，如 **oc** ，它与 Kuberntes 的 kubectl 相当，但有一些不同，如可以从源构建容器映像，并使用单个命令将其部署到环境中，或使用 **odo** 在集群上编写、构建和调试应用程序，而无需管理集群本身。Red Hat OpenShift 支持 Kubernetes 操作员和部署、第三方工具，如 Helm Charts(应用程序部署)、Prometheus(监控和警报管理)、Istio(管理分布式微服务架构)、Knative(无服务器)、内部容器注册表、基于 EFK (ElasticSearch、Fluentd、Kibana)或 Jenkins 的日志堆栈，这些工具可以轻松地使用 CI/CD 管道部署我们的应用程序。我们可以使用单个帐户进行身份验证，使权限管理更容易。Red Hat OpenShift 的一个伟大之处是使用 *Image Stream* 管理容器图像，例如，允许在容器注册表中更改图像的标签，而无需下载图像，在本地标记图像并将其推回。有了 OpenShift，一旦你上传了图片，你就可以在 OpenShift 的虚拟标签中管理它。也可以定义触发器，例如当标签改变其引用时(例如，从 *devel* 到 *stable* 或 *prod* 标签)或如果出现新图像时，触发器开始部署。Kubernetes 和 OpenShift 的另一个区别是基于网络的用户界面。Kubernetes 仪表板需要单独安装，我们可以通过 Kube 代理访问它。Red Hat OpenShift 的 web 控制台有一个登录页面，易于访问，对日常管理工作很有帮助，因为您可以通过表单创建和更改资源。我们可以使用托管服务(IBM Cloud 上的 Red Hat OpenShift、AWS 上的 Red Hat OpenShift 服务、Azure Red Hat OpenShift)在云中安装 Red Hat OpenShift 集群，或者我们可以通过从另一个云提供商(AWS、Azure、Google Cloud、平台无关)安装来自行运行它们。我们还可以在受支持的基础设施(裸机、IBM Z、Power、Red Hat OpenStack、Red Hat 虚拟化、vSphere、Platform agonistic)上创建集群，或者在我们的笔记本电脑上创建对本地开发和测试有用的最小集群(MacOS、Linux、Windows)。

# 为什么红帽 OpenShift 针对 ML/DL？构建生产就绪的 ML/DL 环境

我想每个人都会同意，创建高性能的 ML/DL 模型和在产品中部署 ML/DL 需要不同的技能。为了允许在生产中部署 ML/DL 应用程序，我们需要建立一个迭代过程，从设定业务目标、收集和准备数据、开发模型、部署模型、推理、监控到管理准确性。为了执行这一流程，我们需要实施一个 ML/DL 架构，包括 ML/DL 工具、DevOps 工具、数据管道、对资源(计算、存储、网络)的访问，无论是在私有/公共/混合/多云环境中。Red Hat OpenShift 正在发挥作用，因为它允许数据科学家/开发人员专注于他们的模型/代码，并将其部署在 Kubernetes 上，而无需深入学习 Kubernetes。我们自动化一次，然后我们就开发环境。换句话说，可以管理 ML/DL 模型部署的复杂性，并使允许在任何环境下大规模部署任何容器化 ML/DL 堆栈的技术大众化。易于访问 GPU、FPGAs、x86、IBM Z 或 IBM Power Systems 等专用硬件资源，易于管理硬件驱动程序和库的生命周期。

# 结论

Red Hat OpenShift 有许多功能和优势，可以帮助数据科学家和开发人员真正专注于他们的业务，并使用他们最熟悉的工具和语言。OpenShift 提供了额外的安全控制和工具来管理多个应用程序(多租户环境)。它使所有环境都更易于管理。

**来源**

https://www.openshift.com

【https://en.wikipedia.org/wiki/OpenShift 

[https://www.openshift.com/learn/topics/ai-ml](https://www.openshift.com/learn/topics/ai-ml)

[https://cloud owski . com/articles/10-differences-between-open shift-and-kubernetes/](https://cloudowski.com/articles/10-differences-between-openshift-and-kubernetes/)

[https://docs . open shift . com/container-platform/4.5/CLI _ reference/developer _ CLI _ odo/understanding-odo . html](https://docs.openshift.com/container-platform/4.5/cli_reference/developer_cli_odo/understanding-odo.html)