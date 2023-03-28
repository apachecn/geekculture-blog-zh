# 使用 AWS Lambda、API Gateway 和 DynamoDB 部署的无服务器应用程序

> 原文：<https://medium.com/geekculture/serverless-applications-with-aws-lambda-api-gateway-and-dynamodb-deployed-with-terraform-d7d38c9cf519?source=collection_archive---------7----------------------->

![](img/7f1c12ad03cb12438f7f6e6255ca4fd7.png)

在这个场景中，我们将创建一个无服务器应用程序。我们将使用云服务，如 AWS API Gateway、Lambda 和 DynamoDB，使用 Terraform 部署它们。

lambda 函数将对 DynamoDB 表具有读写权限。

无服务器计算是一种云计算模式，其中云提供商自动管理计算资源的供应和分配。这与用户负责直接管理虚拟服务器的传统云计算形成对比。

需要几样东西:

*   一个 AWS 帐户，在 IAM 中设置一个用户，并将所有密钥等输入 AWS CLI。
*   [地形](https://www.terraform.io/downloads.html)安装完毕

让我们从创建 Lambda 函数开始。我把这个文件命名为 **lambdademo.js**

关于我们使用的表达方式的一点解释。 **await** 可以放在任何基于 async(异步函数) **promise** 的函数前面，以暂停该行代码，直到 **promise** 实现，然后返回结果值。当调用任何返回**承诺**的函数时，可以使用 **await** ，包括 web API 函数。

我们用了 Node。js，一个建立在 Chrome 的 JavaScript 运行时之上的平台，用于轻松构建快速和可扩展的网络应用。节点。js 使用了一个**事件** - **驱动**、**非** - **阻塞**的 I/O 模型，这使得它变得轻量和高效。

现在我们需要压缩文件，创建 **lambdademo.js**

这个压缩文件将被上传到一个 S3 桶。

```
$ zip lambdademo.zip lambdademo.js
```

现在我们将创建一个文件 **assume_role_policy.json，**，在这里我们将授权 lambda 代表我们访问其他 AWS 服务。该文件将包含以下 JSON 对象。

现在，我们将创建一个文件 **policy.json** ，在这里我们将定义 lambda 函数可以执行哪些操作以及在哪个资源上执行。它允许我们的 lambda 函数在名为 **myDB** 的 DynamoDB 表上执行上面定义的操作。

让我们创建我们的 **main.tf** 文件。这设置了我们的提供者、区域、DynamoDB 表等，并展示了我们所有的资源。

我们试驾一下吧。`terraform init`

然后`terraform plan`

运行`terraform apply` 命令后，CLI 中将出现一个 URL，如下所示

```
[https://brqhu55tr8.execute-api.us-east-1.amazonaws.com/Prod](https://brqhu55tr8.execute-api.us-east-1.amazonaws.com/Prod)
```

让我们尝试几个命令来测试我们的应用程序，比如写入 DynamoDB 表。我们将使用 curl 命令来连接。

```
$ curl -X POST -d '{"operation":"write","id":"1","name":"ram"}' https://brqhu55tr8.execute-api.us-east-1.amazonaws.com/Prod/myresource
```

哪个应该返回`{"message": "Item entered successfully"}`

通过运行以下命令执行读取操作:

```
curl -X POST -d '{"operation":"read","id":"1"}' [https://brqhu55tr8.execute-api.us-east-1.amazonaws.com/Prod/myresource](https://brqhu55tr8.execute-api.us-east-1.amazonaws.com/Prod/myresource)
```

然后它应该返回`{"message":"{"Item":{"id":"1","name":"ram"}"}`

恭喜你！我们已经创建并部署了一个无服务器应用程序，它可以使用一个 Lambda 函数在 DynamoDB 上执行读写操作，该函数是通过 API gateway 调用来调用的。

感谢您的阅读！