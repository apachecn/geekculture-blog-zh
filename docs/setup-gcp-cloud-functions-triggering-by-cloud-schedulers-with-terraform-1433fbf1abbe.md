# 使用 Terraform 设置由云调度程序触发的 GCP 云功能

> 原文：<https://medium.com/geekculture/setup-gcp-cloud-functions-triggering-by-cloud-schedulers-with-terraform-1433fbf1abbe?source=collection_archive---------11----------------------->

这是一个一步一步的教程，介绍如何设置 GCP 云功能，通过云调度程序自动触发它们，所有这些都是用 Terraform 完成的。

![](img/0363cd223cd237314b647bd5a54cef62.png)

# 要求

*   [GCP 账户](https://console.cloud.google.com/)
*   [Terraform CLI](https://www.terraform.io/downloads.html) 本地安装，矿用 1.0.6。

# GCP 构型

你可能已经有了一些必要的配置，检查以确保你没有遗漏任何东西。

## 创建服务帐户(SA)

顾名思义，这是一个其他服务用来在 GCP 应用配置的帐户。每个 SA 可以有多个角色，这些角色为其密钥持有者提供必要的权限。我们将使用这个 SA，姑且称之为*教程-sa* 通过 Terraform 创建资源，并使用云调度器调用云功能。

出于本教程的目的，将以下角色分配给*教程-sa* :

*   Cloud Functions Admin :它包括 Cloud Functions . Functions . getiampolicy 角色，我们需要这个角色来创建一个功能化的云函数。稍后你会看到它的细节。
*   **云功能开发者**:部署、更新、删除功能
*   **云函数调用器**:能够调用一个函数
*   **云功能服务代理**
*   **云调度管理**

向它添加一个 json 密钥，并将密钥保存在安全的地方。

## 创建地形桶(可选)

如果您想将 Terraform 状态文件存储在远程存储上，而不是本地机器上，您需要事先在 Google 云存储(gcs)上创建一个 bucket。

在您的 GCP 项目中搜索云存储，并创建一个存储桶。存储桶名称必须唯一。

## 启用 API

所有资源的创建、更新和删除都是通过一组 API 调用来完成的。在 GCP，我们需要预先启用这些 API，您最终可以从 dashboard 禁用它们。要激活 API，请在您的 GCP 仪表板顶部栏搜索区域键入名称，从下拉列表中选择市场上要转移到相应页面的 API，然后单击启用按钮。您需要启用以下 API:

*   云构建 API
*   云调度程序 API
*   计算引擎 API
*   云函数 API

您可以在 GCP*API&服务仪表板中检查所有启用的 API。*

# 地形结构

在您最喜欢的 IDE 中打开一个空文件夹，我将使用 Visual Studio。我建议你在你的 IDE 上安装一个 Terraform 扩展，它将帮助你语法高亮和自动完成。在 Visual Studio 上，您可以继续使用 *Hashicorp Terraform。*

添加一个新的 terraform 文件，我将把它命名为 *backend-config.tf* 放到你的空文件夹中，并粘贴以下内容。

*地形块*用于配置地形本身。让我们探索它们，

```
terraform {
  backend "gcs" {
    bucket  = "<bucket-name>"
    prefix  = "state"
  } required_version = ">= 0.12.7" required_providers { 
    google = {
      source = "hashicorp/google"
      version = "3.82.0"
    }
  }
}provider "google" {
  project = "<gcp_project_id>"
  region  = "<regione_name>"
  zone    = "<zone_name>"
}
```

## Terraform 后端配置(可选)

在 Terraform 块内部，我们也可以嵌入我们的后端配置。Terraform 使用后端配置来确定在哪里存储*状态*文件，以及在哪里运行操作(API 调用)。如果您不配置后端，Terraform 将使用其默认行为，这意味着:

*   Terraform 将把它的状态文件存储在你的本地，直接在当前工作区内而不是像 GCP 水桶那样远程存储。
*   Api 调用基础设施服务(操作)来操纵云功能等资源，将从本地机器而不是 Terrafrom Cloud 或 Terrafom Enterprise 完成。

我们的配置将地形状态文件存储在 GCP 桶中，并从本地机器进行操作。

*gcs* 后端还支持*状态锁，*意味着当对资源进行更改时，它将锁定状态文件，因此其他人无法进行更改。

## Terraform 所需版本

我们可以在操作资源时对要使用的 terraform 版本添加一个约束。如果 Terraform 的运行版本不满足这一要求，它将产生一个错误，而不采取任何行动。

设置所需的版本来保存状态文件以免损坏是一个很好的做法。

## 提供商要求

为了与 CGP 这样的远程系统协同工作，Terraform 依赖于名为*provider 的插件。*为了启用这些提供程序，我们应该将它们添加到 Terraform 块中。

提供者需求包括一个本地名称，一个告诉 Terraform 可以从哪个注册中心下载插件的源位置，以及一个版本约束。提供程序版本是可选的，如果没有设置，将使用最新的版本。对于我们的谷歌提供商，你可以查看[这个](https://registry.terraform.io/providers/hashicorp/google/latest)链接。点击 us *使用提供商*按钮获取配置。

现在让我们配置提供者本身。我们需要传递我们的 GCP 项目 Id，地区和区域的资源将被创建。例如，我将欧洲-西方 1 作为区域，欧洲-西方 1–a 作为区域。您可以使用 Terraform 变量动态传递这些值。

查看此[链接](https://cloud.google.com/compute/docs/regions-zones/viewing-regions-zones)以获取 GCP 可用地区和区域的列表。为此，您必须启用计算引擎 API。

# 地形云函数

谈到资源，Terraform 有一个很好的文档。[这里的](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/cloudfunctions_function)是 GCP 的云函数。

CCP 云函数可以从桶中取出代码来运行。我们将在一个桶上存储代码项目，并配置我们的云函数来使用它。为此，我们需要创建:

*   桶资源
*   保存我们代码的桶对象资源
*   云函数本身

让我们创建一个名为 main.tf 的文件，并添加我们的资源。

## 水桶

bucket 只有一个名字，*记住这个名字必须是唯一的*。您也可以使用我们为 terraform 创建的桶来避免创建新的桶。

```
resource "google_storage_bucket" "bucket" {
  name = "<unique-bucket-name>"
}
```

## 桶对象

GCP 云函数支持包括 Go、Node.js、Java、Python 等多种语言。它可以加载一个包含要运行的代码项目的 zip 文件夹。有了一个由云函数运行的 Node.js 应用程序，我们至少需要将 package.json 和一个 javascript 文件放在 zip 文件夹中。让我们使用现有的样本 Hello World 项目作为 GCP 云函数的一个例子，做一点小小的改动。

让我们创建一个 index.js 文件，用一个简单的 hello 来响应。稍后我们将看到在哪里传递环境变量。

```
exports.helloWorld = (req, res) => {
  let name = process.env.name;
  let message = `Hello ${name}`;
  res.status(200).send(message);
};
```

和一个 package.json 文件，其中包含:

```
{
"name": "sample-http",
"version": "0.0.1"
}
```

创建一个包含 index.js 和 package.json 的 index.zip，放在我们项目的根目录下。现在让我们创建 bucket 对象，

```
resource "google_storage_bucket_object" "cloud-function-archive" {
  name   = "index.zip"
  bucket = google_storage_bucket.bucket.name
  #relative path to your index.zip file from the working directory
  source = "./index.zip" 
}
```

桶对象，获取

*   对象名称，
*   桶名把文件放在里面，如果你在上一步创建了“*Google _ storage _ bucket”“bucket”*资源，可以通过*Google _ storage _ bucket . bucket . name*作为引用，否则只放一个带有你已经创建好的桶名的字符串。
*   index.zip 文件的相对路径

## 云函数

现在是我们的云函数的时候了

```
resource "google_cloudfunctions_function" "function" {
  name        = "terraform-tutorial"
  description = "Hello World example"
  runtime     = "nodejs14"

  available_memory_mb   = 128
  # Gets a string bucket name or a reference to a resource
  source_archive_bucket = google_storage_bucket.bucket.name
  source_archive_object = google_storage_bucket_object.cloud-function-archive.name trigger_http = true
  entry_point  = "helloWorld"
  environment_variables = {
    name= "terraform"
  }
}
```

名称和运行时间是必需的。注意运行时被设置为 *nodejs14* ，你可以在这里获得所有可用的运行时[。](https://cloud.google.com/functions/docs/concepts/exec#runtimes)

如果您在前面的步骤中创建了一个"*Google _ storage _ bucket " " bucket "资源，那么请将 name 属性作为引用传递给 source_archive_bucket，否则就放入一个带有 bucket 名称的字符串。也为 bucket 对象添加一个引用。*

Trigger_http 设置为 true 意味着该函数将通过对其端点的 http 调用来触发。稍后我们将使用*云调度器* 通过 http 调用来触发该功能。Post、Get、Put、Delete 和 Options 都支持 http 调用。

顾名思义，entry_point 是触发云函数时执行的代码中的函数名。

我们还为云函数传递环境变量，如代码块所示。这是我们在 index.js 文件中使用的变量。

现在让我们转到云函数的调用管理部分

```
resource "google_cloudfunctions_function_iam_member" "invoker" { project = google_cloudfunctions_function.function.project
  region = google_cloudfunctions_function.function.region
  cloud_function = google_cloudfunctions_function.function.name role   = "roles/cloudfunctions.invoker"
  member = "serviceAccount:<terraform_sa_email>"}
```

对于我们的云函数调用，有两个策略。所有用户授权或单个用户授权，我们选择后者。这个配置只允许我们的 terrfaorm-sa 调用这个特定的云函数。

选择 AllUsers 策略后，云函数调用将对所有成员公开。

# 使用 Terraform 的云调度程序

云调度程序运行作业。在我们的例子中，我们希望创建一个作业来触发对我们的云函数的 http 调用。[这里的](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/cloud_scheduler_job)是关于这个资源的文档。

## 创建应用引擎应用

GCP 应用引擎是我们可以运行*无服务器应用*的地方。为了有一个云调度程序，我们的 GCP 项目必须有一个应用引擎应用程序。进入应用引擎，点击创建应用。无需进一步配置。

## 云调度程序

我们将创建一个作业，每 2 分钟触发一次对我们的云函数的 http 调用。将以下内容复制并粘贴到 main.tf 中

```
resource "google_cloud_scheduler_job" "hellow-world-job" {
  name         = "terraform-tutorial"
  description  = "Hello World every 2minutes"
  schedule     = "0/2 * * * *"
  http_target {
    http_method = "GET"
    uri = google_cloudfunctions_function.function.https_trigger_url
    oidc_token {
      service_account_email = "<terraform-sa-email>"
    }
}
```

注意 http_target，uri 属性获得了对 main.tf 文件中的云函数资源的引用。

我们来谈谈 oidc_token。我们在对 GCP 端点进行 API 调用时使用这种令牌类型，这些端点的*不是以*.googleapis.com 结尾的*。因为我们的云函数 trigger_url 不是其中之一，所以 oidc_token 适合我们。

这个云调度程序有权限调用我们的云函数，因为之前我们配置了云函数 Invoker Iam 策略来接受 *terraform-sa* 服务帐户 http 调用。

# 让我们运行它

有一系列步骤来初始化和运行我们的项目。

## 地形初始化

该命令初始化 terraform 目录，它下载提供程序并将其存储在一个名为. terraform 的隐藏目录中。它还创建状态文件。如果 Terrafrom backend 像 gcp 一样设置为 remote，那么文件将在配置的 bucket 中，否则您将在您的工作目录中看到它。在运行该命令之前，我们必须确保我们的 GCP 提供者被授权对 GCP 项目进行 API 调用。

**GCP 提供商授权**

我们通过向 Terraform 提供我们的服务帐户密钥来授权我们的 GCP 提供商。为此，我们需要设置一个环境变量。

```
export GOOGLE_APPLICATION_CREDENTIALS="<absolute_path_to_service_account_key>"
```

现在让我们再次尝试 *terraform init，*它将成功初始化 terraform。检查我们的 GCP 存储桶，我们将看到在我们定义的目录路径下， *default.tfstate* 文件被存储。

## 地形图

这个命令就像客户端的一次演习，这意味着它显示了对基础设施的所有更改。如果有语法错误，这个命令会指出来。

## 地形应用

现在让我们应用我们的更改。Terraform 将创建四个资源，你会看到它们被一个接一个地创建。

我们结束了。

# 结果验证

在 GCP 仪表板上，您将有一个成功运行云功能的云调度程序。检查调度程序上的触发 url 以及云函数上的认证部分，以查看我们在 Terraform 端设置的属性的效果。

您还可以查看调度程序和云功能的日志，以了解成功结果。

如果要保留资源，暂停云调度程序，这样它就不会每两分钟调用一次云功能。

[这里的](https://github.com/rzeAkbari/terraform-tutorial/tree/master)是该项目的 Github 链接

# 打扫

打扫卫生

## 地形破坏

运行 terraform 销毁命令。如果您在 GCP 仪表盘上手动更改了 terraform 创建的资源，运行 destroy 命令时可能会出现错误。这是因为现在地形状态文件和基础设施资源之间存在差异。

您可以回滚到 GCP，或者通过更改文件中的 json 值来手动修复状态文件，或者从文件中完全删除有问题的资源，并最终从 GCP 仪表板中手动删除您的资源。

你可以在这里阅读更多关于 *Teraform destroy* 的内容:[https://space lift . io/blog/how-to-destroy-terraform-resources](https://spacelift.io/blog/how-to-destroy-terraform-resources)

## 移除桶

所有手动创建的资源，包括 Terraform 状态文件存储桶，也应该手动删除。

## 禁用 API

您还可以禁用我们在开始时启用的所有 API。据我所知，API 调用在某些特定的配额内是免费的，尽管我还没有检查它们的所有成本细节。