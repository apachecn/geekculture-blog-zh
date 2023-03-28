# 将文件加载到 AWS S3 存储桶的最快方法:亚马逊命令行界面使用指南

> 原文：<https://medium.com/geekculture/the-fastest-way-to-load-files-into-aws-s3-buckets-a-guide-to-using-amazon-command-line-interface-26e38982a702?source=collection_archive---------3----------------------->

如果您曾经处理过大量的数据，那么您很有可能在某种程度上使用过 AWS。由于其高耐用性和可用性，难怪全球超过 32%的公共云份额由其基础架构托管。

AWS S3(简单存储服务)是亚马逊提供的最广泛使用的服务。通过提供对象存储，它可以托管最大 5tb 的文件。这就引出了一个问题，当处理如此大量的数据时，**移动数据的最快方法是什么**？答案是命令行界面(CLI)。

利用 CLI 上传、复制和同步文件可节省时间和资源，并使在存储桶和本地目录之间移动数据变得容易。我已经使用 AWS CLI 为您的数据需求编写了一个快速有效的指南。

![](img/5976fdc81c1cdabd109e14c7021b9521.png)

CLI can upload, download and delete files from S3 quickly and efficiently

**配置 AWS CLI**

首先，下载并安装适用于 [Windows](https://awscli.amazonaws.com/AWSCLIV2.msi) 、 [Linux](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip) 或 [Mac](https://awscli.amazonaws.com/AWSCLIV2.pkg) 的 AWS CLI 安装程序。安装完成后，打开命令提示符并键入“aws configure”。

这样做之后，系统会提示您输入您的 S3 存储桶密钥。通过登录 AWS 管理控制台并访问 IAM 控制台，可以找到访问密钥 ID 和秘密访问密钥 ID。我在下面的例子中使用了虚拟键来展示它们的样子。

接下来，输入默认区域和输出的首选格式。我个人使用文本，但支持以下所有内容:

*   json
*   yaml
*   yaml 流
*   文本
*   桌子

aws configure example

**S3 排行榜**

在执行任何操作之前，我检查目标桶的内容。您可以通过使用“ls”来这样做，这将输出指定桶的所有内容。

ls will output a list of the current bucket’s contents

**复制/上传文件**

CLI 功能的基础是“cp”(复制)。现在，在存储桶和本地机器之间复制数百、数千甚至数百万个文件都可以轻松完成。我遇到的最常见的用例是将文件从本地目录加载到 S3 桶中。但是，也支持将文件从 S3 复制到本地目录。

该功能无需手动使用 S3 界面中的上传按钮，只需很少的时间即可完成上传。

向代码中添加额外的函数(- options)如 include、exclude 和 recursive 是可选的，但是功能强大。通过添加这些文件，您可以排除和选择多个要一次复制、删除或同步的文件。稍后将更详细地介绍 CLI 选项。

Copy files from a local directory to a S3 bucket

**删除对象和存储桶**

要轻松清理 S3 桶，命令行功能“rm”特别有用。此函数将从选定的目标存储桶中删除特定的对象。要删除整个 S3 存储桶，可以使用“rb”功能，但该存储桶必须先为空。与 copy 命令(“cp”)一样，添加附加选项可以一次删除多个文件，删除带有特定前缀的文件，或者删除存储桶中的文件类型和文件。

delete an object from S3 with rm

CLI 选项

为了充分利用 AWS CLI，可以添加可选参数。当引用一个可选函数时，记得用双破折号("-")来引导函数。

三个最重要和最常用的选项是:

*   递归:调用该选项时，将对指定存储桶中的所有文件执行该命令。
*   包含:此选项会将命令扩展到所有符合您已声明的指定标准的文件。最常见的用例是选择某一类型的所有文件，例如下面的例子，其中我包括了所有文件。csv 文件。
*   Exclude:当调用该选项时，它将从所有声明的文件中排除该命令的操作。

Example CLI code that will copy all csv files from the source to the target bucket

您可以同时使用多个选项来确保所需的效果，但是这并不是成功执行的必要条件。

**-试运行:测试你的代码**

对你的代码进行最终的测试运行总是最佳实践。Dryrun 是您使用 CLI 实现这一目标的工具。在我执行任何我没有完全把握的命令之前，我会在我的命令后面加上“- dryrun”。这样做将使 CLI 执行您的命令，并向您显示结果**，而没有任何影响**。这是一个很好的方法来试运行您的第一个操作，并确保您不会对目标 S3 存储桶产生任何意外的后果。

在下面的示例代码片段中，目的是删除目标 S3 存储桶中除已处理文件夹之外的所有文件。预演可以让我看看我的命令是否正确！

Running a dry run of deleting all objects in the targeted S3 bucket except the processed folder

**收尾思路&附加应用**

AWS 命令行界面帮助我的团队在几分钟内加载了数千个文件，而不是几个小时。几行代码可以节省时间、金钱和计算资源。在研究如何加速将文件加载到 S3 存储桶时，我很难找到资源和清晰的答案。我希望这个指南能为你提供一些快速的答案。

**最后一个注意事项**

AWS CLI 可用于与 Amazon 的许多服务产品配合使用，以提高和加快性能。可以在下面找到 CLI 支持的服务的完整列表:

*   DynamoDB
*   EC2(弹性云计算)
*   S3 冰川
*   识别和访问管理
*   简单通知服务
*   简单工作流服务

完整的 AWS CLI 文档可以在[这里](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html)找到。

一如既往的感谢您的阅读！你可以在推特上通过 [@ZachEng1ish](https://twitter.com/ZachEng1ish) 联系我，并在这里找到我以前的故事[。](https://zach-english.medium.com/)