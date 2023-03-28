# AWS ECS Anywhere 快速入门

> 原文：<https://medium.com/geekculture/aws-ecs-anywhere-quick-start-b03c1449af62?source=collection_archive---------22----------------------->

当我们谈论数字化时，首先总是指云迁移，但我们不可否认的是，混合云用例在许多领域仍然需要，例如边缘计算资源、物联网机群管理、需要部署在用户附近的低延迟应用程序。AWS ECS Anywhere 是一款产品，AWS 允许我们管理 AWS 外部的实例，并使用相同的 ECS 控制平面管理容器的生命周期和运行状况。今天，我们将在任何地方试用 ECS，并讨论在任何地方采用 ECS 时您要考虑的领域。

# AWS ECS Anywhere 高级流程

![](img/03798af04bfc01505dd632f579c5f76b.png)

演示虚拟机或者裸机 Linux 环境，这个演示我们会用 vagger 搭建一个 Ubuntu Linux 盒子。

# 演示环境设置

按照以下设置准备演示环境。

## 安装虚拟箱

我们将为我们的演示环境安装 [VirtualBox](https://www.virtualbox.org/) ，按照官方指南下载 VirtualBox，或者如果你使用 Mac 机，可以用`brew`安装:-

```
brew install virtualbox
```

## 安装流浪者

我们将使用流浪者来管理我们的虚拟环境，从[流浪者](https://www.vagrantup.com/)官方网站安装，或者使用`brew`安装，如果你使用 Mac 机:-

```
brew install vagrant
```

## 启动虚拟机

在您的开发环境中创建一个名为`ecs-lab`的文件夹，并运行以下命令

```
vagrant init ubuntu/bionic64
vagrant up
```

![](img/2dd9126081247b210fc6511b80503a6f.png)

您应该从日志中看到图像正在下载并调出环境。

## 暴露端口

编辑流浪者文件，并将以下行添加到文件中:-

```
config.vm.network "forwarded_port", guest: 8080, host: 8080
```

您应该在配置部分添加如下内容:-

![](img/4e8375a58729d42a107c75c8773f4196.png)

运行命令`vagrant reload`来更新虚拟环境，您应该看到它暴露了如下所示的端口`8080`

![](img/5b0db368ba7ffe4df62dd2151e8c498a.png)

## 登录 Ubuntu

运行命令`vagrant ssh`登录虚拟机。要注册 ECS 实例，您需要运行`root`模式，运行`sudo su`进入超级用户模式。

![](img/35e1a99a09cac066c48efba7b36cbb3b.png)

# 设置 ECS 集群并注册外部实例

我们已经设置了虚拟环境，现在我们需要创建一个新的 ECS 群集并获取注册设置脚本。

## 创建 ECS 集群

转到 ECS 控制台，使用仅联网选项创建一个群集，该选项用于`AWS Fargate or External instance capacity`。

![](img/97355bc09d69c3ff468c339fe2c9679b.png)

在这个步骤中，给它一个名字`default`，不要点击这个选项。完成创建集群的步骤。

![](img/4c6fec59d6fe4e5dd23453820f5a1037.png)

## 注册外部实例

进入你的集群页面，转到`ECS Instances`选项卡，你会看到一个选项`Register External Instances`。

![](img/02da9b91e3bd34eac4d0e1c2a5e43f69.png)

点击`Register External Instances`进入注册页面。

![](img/cbd1c9112e1369faa8ade43fa3139814.png)

在`Register external instances`页面中，按照以下选项点击`Next step`。

![](img/6e4e55abb3c5515d956a8f9ee7d44a76.png)

在步骤 2 中，页面将提供 shell 命令来注册外部实例。复制脚本并在`Vagrant terminal.`中运行

![](img/ae43d9b050d3cf7915863be0e9c68e56.png)

该脚本将下载注册实例所需的组件和依赖项。等到它结束。

![](img/473db8af443977cc46edc955f69b8645.png)![](img/3a2abadc41f96834fe8c9b5078c8148a.png)

完成后，您可以运行`docker ps`来检查 ECS 代理是否正在运行。

![](img/39e6636ca2e94b3d9601edfe61c63f50.png)

返回 ECS 控制台页面，您应该看到一个 ECS 实例已注册，单击该实例可了解更多详细信息。

![](img/695a0e86fd9c9df1696e025df015410c.png)

# (附加)启动从 AWS 到实例的远程会话

AWS 利用 [AWS Fleet Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/fleet.html) 来管理实例，您可以从 AWS 控制台启动一个远程会话来对实例进行故障排除，而无需 SSH 到您的实际机器。

单击以`mi-.....`开始的实例 id，它会将您带到车队管理器页面。

![](img/b13ca92350f74cc1138efc3410d764b5.png)![](img/687e4442af4ca25f71812820b6533a2f.png)

当你点击`Instance actinons`中的`Start session`时，它将激活会话管理器

![](img/f1f0263e95cba550d2176f1bf208a13c.png)![](img/10a8c0a9c5563a4c2dee0653596e0ef0.png)

# 创建 ECS 服务

我们将创建一个 ECS 服务，在新注册的实例中运行。

## 创建新的任务定义

使用以下设置创建新的 Nginx 任务定义。创建外部启动类型的新任务定义。

![](img/5986f7e1b32ab48e9fa95385d5882d52.png)![](img/ce86fe6e22472423f6d2f57ae63fc274.png)

为任务和任务执行使用正确的角色，这里的[可以参考](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)。

![](img/23a84ff29c94a33fc8925c863e4a6455.png)

添加具有以下设置的容器。

![](img/0e5924dfbaf88bb5e064277200bce18c.png)![](img/65b63c037ec525241819e8b92ff94616.png)![](img/7130735c617fcc45bb51166aee515c84.png)

## 创建 Nginx 服务

在任务定义之后，我们应该使用下面的任务创建一个新的服务。

![](img/8b2f19b676d5eac517eee4c3e353c239.png)![](img/7ce3c02837cf7b3ca5f93c3030f0ee71.png)![](img/ea06ec4bbb43d41c1f9236ce2f956d7a.png)![](img/9c899f269fadbfc9ce61fa76d5160795.png)![](img/25ddc1f2a2bc9a1973bce71b766dfd27.png)

几秒钟后，您应该会看到服务已经启动，并且能够在控制台中查看日志。

![](img/4b857f398ad81ede54eb15f33daae6e2.png)

## 测试 Nginx

现在我们可以用`http://localhost:8080`浏览 Nginx-Service local

![](img/6194b504186be901a1a0d14fdb272cd1.png)

# 打扫

在您对研究感到满意之后，您可以从 ECS 实例页面中取消注册实例，并运行命令`vagrant destroy`来清理虚拟机。

# 讨论

## 用例

ECS Anywhere 使用相同的 ECS 控制平面简化了容器管理，降低了在本地环境中管理容器的要求。如果您现有的团队已经在容器管理中使用 ECS，您可以使用相同的工具和 CICD 工作流来管理数据中心的内容。

## 出站连接要求

ECS Anywhere 需要与 AWS 平台的稳定连接，以下出站连接是`ap-northeast-1`区域所需的，由 ECS API 和代理连接、CloudWatch API、S3 端点、SSM、EC2 消息 API 和 AWS Fleet API 组成，如果您的环境有严格的规定，您需要提交以下列表以供审查和加入白名单:-

*   ecs.ap-northeast-1.amazonaws.com。
*   ecs-t-*。ap-northeast-1.amazonaws.com。
*   ecs-a-*。ap-northeast-1.amazonaws.com。
*   ec2messages.ap-northeast-1.amazonaws.com。
*   ssm.ap-northeast-1.amazonaws.com。
*   s3.ap-northeast-1.amazonaws.com。
*   ssmmessages.ap-northeast-1.amazonaws.com。
*   amazon-ecs-agent.s3.amazonaws.com。
*   AWS-fleet-manager-artifacts-AP-northeast-1 . S3 . AP-northeast-1 . Amazon AWS . com
*   s3-r-w.ap-northeast-1.amazonaws.com。
*   s3-w.us-east-1.amazonaws.com。

## Ubuntu 和 Docker 出站要求

尽管具体情况具体分析，但是如果您的机器是 Ubuntu，并且您的 docker 映像存储在 Docker 中，那么您还需要以下出站连接

*   archive.ubuntu.com。
*   security.ubuntu.com。
*   download.docker.com。

## 支撑平台

在撰写本文时，ECS Anywhere 仅支持 Linux 环境。尽管 ECS 代理支持 Window 平台，但外部实例注册目前不支持 Window。希望今年我们能看到这一点。在撰写本文时，只有以下 Linux 发行版支持

*   CentOS
*   RHEL
*   一种男式软呢帽
*   OpenSUSE
*   人的本质
*   一种自由操作系统

## 限制

如果将 ECS Anywhere 与 Docker Swarm 或 Kubernetes 相比较，有许多空白需要填补，例如 ask 服务网格、负载平衡、服务发现和秘密。

## 费用

ECS Anywhere 对每个托管注册实例每小时收费 0.01025 美元。

# 拿走

容器应用程序很简单，但是管理容器应用程序很有挑战性。当考虑在本地环境中管理容器的使用情形时，除了 Kubernetes 或 Docker Swarm 的高要求设置之外，AWS ECS Anywhere 将是管理容器和将 ECS 控制平面扩展到您的数据中心的一个简单和轻量级的选项。