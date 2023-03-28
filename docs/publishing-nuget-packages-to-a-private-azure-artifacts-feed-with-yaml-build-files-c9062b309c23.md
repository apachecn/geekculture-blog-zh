# 将 NuGet 包发布到带有 YAML 构建文件的私有 Azure 工件提要。

> 原文：<https://medium.com/geekculture/publishing-nuget-packages-to-a-private-azure-artifacts-feed-with-yaml-build-files-c9062b309c23?source=collection_archive---------12----------------------->

## 我们可以通过 YAML 文件中定义的管道轻松地将 NuGet 包发布到 Azure 工件中托管的内部提要。

![](img/e60fd8b4669bab8c3a2805236cfb3e41.png)

使用 Azure 工件，我们可以将 NuGet 包发布到私有(或公共)NuGet 提要。这些提要可以在 Azure DevOps 中在组织级别或项目级别确定范围。

在 Azure DevOps 中创建一个私有的 NuGet feed 非常简单。下面这篇文章展示了你可以设置一个。如果你正在跟进，并且还没有建立一个内部 feed，停止阅读这篇文章，看看下面的文章。一旦你完成了，你可以回到这里。

[](https://docs.microsoft.com/en-us/azure/devops/artifacts/get-started-nuget?view=azure-devops&tabs=windows) [## NuGet 包入门- Azure 工件

### 开发人员可以使用 Azure 工件在提要和公共注册中心之间发布和使用 NuGet 包…

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/devops/artifacts/get-started-nuget?view=azure-devops&tabs=windows) 

这篇文章将向您展示我们如何使用 YAML 构建文件将我们创建的 NuGet 包发布到 Azure 工件中我们自己的私有提要。

现在你可能对 YAML 有一个总的看法，是的，我们可以通过经典的用户界面获得相同的结果，但是我喜欢能够在我们的代码库中包含我们的构建文件，并且让我能够使用那个 YAML 文件的 git 历史来检查我们的构建文件的历史。

要将我们的 NuGet 包发布到我们的内部提要，我们需要执行以下操作:

*   创建我们的 NuGet 包
*   决定我们包的版本策略
*   将我们的包发布到我们的内部订阅源

在这篇文章中，我将使用我在自己的健康应用程序中使用的助手库。如果你在阅读这篇文章的时候想看看它(它包括 YAML 的文件)，看看这个:

[](https://github.com/willvelida/MyHealth.Common) [## willvelida/MyHealth。普通的

### Permalink 无法加载最新的提交信息。适用于我的所有 MyHealth 应用程序的公共帮助程序库

github.com](https://github.com/willvelida/MyHealth.Common) 

**创建我们的 NuGet 包**

在进入构建文件的主体之前，我们需要设置一些东西:

让我们来分解一下:

*   每当我们提交到我们的*主*分支时，我们就触发构建开始。
*   我们设置了一些在整个构建过程中需要的公共变量。
*   我们使用 Linux 映像作为我们的构建代理。
*   然后我们安装。NET Core SDK，使用运行 restore 命令的 **DotNetCoreCLI** 任务来恢复我们的项目，然后我们使用运行 build 命令的另一个任务来构建项目。

为了在我们的构建文件中创建一个 NuGet 包，我们需要添加一个 **DotNetCoreCLI** 任务，如下所示:

在这个任务中，我们运行 pack 命令，并告诉任务打包我们的项目路径，这个路径是我之前在 YAML 文件中定义的。我已经将 **nobuild** 参数设置为 true，因为我已经构建了我的项目。

我的包是一个. NET 标准包。*为。网芯和。NET 标准包*，微软推荐你使用 **DotNetCoreCLI** 任务。如果你正在为……构建*包。NET Framework* ，可以使用一个 **NuGet** 任务。

[](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/package/nuget?view=azure-devops) [## NuGet 还原、打包和发布任务- Azure 管道

### Azure Pipelines | Azure devo PS Server 2020 | Azure devo PS Server 2019 | TFS 2018 注意，NuGet 身份验证任务是…

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/package/nuget?view=azure-devops) 

**版本化策略**

有几种方法可以做到这一点。在我的包裹里。 **csproj** 文件，我已经像这样指定了我的版本号:

如果我们想通过在 YAML 定义的构建任务来实现这一点，我们可以这样做:

有了 **NuGet** 任务，我们就可以用 **Major 了。Minor.Patch** 我们构建的语义版本化方案。然而，一旦产生了一个版本，我们不能更新或替换该版本。它们是不可改变的。为了在每次更新时生成新版本的包，我们可以执行以下操作:

1.  使用 **$(版本:。r)** 我们希望增加的版本号的变量。这将在我们每次推进到我们的分支时自动增加该变量的构建号，同时保持其他变量不变。
2.  使用 **$(日期:yyyyMMdd)** 变量。这是为构建创建预发布标签的理想选择，同时允许我们保持主要版本、次要版本和补丁版本不变。

**发布我们的包给 feed**

现在我们有了一个包，我们可以将它发布到我们的 feed。为了发布到我们的 Azure 工件提要，我们需要将**项目集合构建服务**身份设置为提要上的**贡献者**。

[](https://docs.microsoft.com/en-us/azure/devops/artifacts/feeds/feed-permissions?view=azure-devops#azure-artifacts-settings) [## 使用订阅源权限管理包- Azure 工件

### 您在 Azure 工件中托管的包存储在一个提要中。通过设置订阅源的权限，您可以共享您的…

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/devops/artifacts/feeds/feed-permissions?view=azure-devops#azure-artifacts-settings) 

一旦我们实现了这一点，我们就可以将下面的 YAML 添加到我们的构建管道中。

这里，我们使用 **NuGetAuthenticate** 命令来认证我们的构建服务器，以将包推送到我们的内部 NuGet 提要。然后，我们使用 push 命令运行一个 **NuGetCommand** 任务，将我们的包(存储在工件暂存目录中)推送到我们的内部 NuGet 提要。

在这里，我们使用 *<项目名称> / <提要名称>* 作为我们的 VSTS 提要来发布到。请记住，在 Azure DevOps 中，我们可以在项目级别或组织级别确定提要的范围。因为我的提要是在项目级别上，所以我需要在这里输入项目名称。

我还包含了参数 **nuGetFeedType** ,并声明我们的目标字段是一个内部提要。因为我们在 NuGet 命令任务中使用了 **push** 命令，所以这个参数是必需的。

**全 YAML 档案**

这是我们完整的 YAML 档案:

**结论**

希望您能看到将我们的 NuGet 包发布到 Azure 工件中的私有 NuGet 提要是多么简单。这不仅仅局限于私有的 NuGet feeds，我们也可以发布从 DevOps 到 NuGet 的包。

如果你想了解更多关于 Azure 工件的知识，我建议你查阅文档，这些文档涵盖了诸如配置提要、发布 NuGet(和其他)类型的包等主题:

[](https://docs.microsoft.com/en-us/azure/devops/artifacts/?view=azure-devops) [## Azure 工件文档

### 只需编写一次代码，就可以在整个组织中共享软件包。托管您的私有 NuGet、npm、Maven、Python 和 Universal…

docs.microsoft.com](https://docs.microsoft.com/en-us/azure/devops/artifacts/?view=azure-devops) 

如果你对这篇文章有任何问题，请在评论中告诉我，或者你可以通过 [Twitter](https://twitter.com/willvelida) 联系我。