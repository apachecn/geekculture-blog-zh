# 自动气象站云形成 vs 地形 vs 自动气象站 CDK

> 原文：<https://medium.com/geekculture/aws-cloudformation-vs-terraform-vs-aws-cdk-4af897c4bb4b?source=collection_archive---------3----------------------->

## 比较 AWS 上最常见的 IaC 工具

正如我在[上一篇关于基础设施即代码(IaC)](/geekculture/what-is-infrastructure-as-code-iac-a10fef64f3b3) 的文章中提到的，IaC 是每个技术人员的武器库中一个非常重要的工具和概念。它是通过机器可读文件(如代码)管理和配置 It 基础架构的过程，而不是物理或手动部署它们。

您可以使用 IaC 在配置文件中定义基础结构，该文件包含组件所需的规范、设置和其他信息。这个文件作为一个模板，可以用同样的方法重新部署多次。

IaC 还有许多好处，如提高部署速度、一致性和减少错误、版本控制、节约成本等等。

在使用 AWS 时，可以使用多种 IaC 工具，但我们将在本文中主要介绍 AWS CloudFormation、AWS Cloud Development Kit (CDK)和 Terraform。

## 自动气象站云形成

AWS CloudFormation 是 AWS 的第一个也是最老的 IaC 工具，发布于 2011 年。CloudFormation 配备了对 JSON 和 YAML 模板文件的支持，这是使用 AWS 时部署 IaC 最常用的方法之一。与所有 IaC 工具一样，它允许您轻松地部署、管理、更改和销毁基础架构中的资源，并且还提供了许多强大的功能，使其对每个人都非常有用。

CloudFormation 要求您创建或利用现有的模板，该模板指定了您想要部署的资源。这个模板可以是 JSON 或 YAML 格式。您可以使用像`Parameters, Mappings, Resources and Outputs`这样的节头在模板中定义您的资源和其他属性。从模板创建的资源集合称为堆栈。

CloudFormation 还支持资源提供者，允许将第三方资源和工具集成到您的堆栈中，例如 Datadog 和 JFrog。

一些利用 AWS CloudFormation 的组织包括 GoDaddy、Expedia 和巴塞罗那足球俱乐部。(来自 AWS 网站)

## 将（行星）地球化（以适合人类居住）

Terraform 于 2014 年推出，是该列表中唯一一个非 AWS 原生的工具。Terraform 不仅可以在 AWS 中使用，还可以在其他云提供商如谷歌云平台和微软 Azure 中使用。它的主要卖点是它赋予开发者使用一个由模块和提供者组成的大型生态系统的能力。

Terraform 使用自己的 Hashicorp 配置语言(HCL ),这基本上是一种更加人性化的 JSON，不过如果您愿意，也可以使用 JSON。

Terraform 有一个非常强大的社区，它汇集了 1700 多个提供商来管理几乎所有类型的资源和服务。这使得 Terraform 用户只需一个工具就可以管理各种各样的资源，而不必使用多个工具。所有可用的提供者都可以在 Terraform 注册表中找到。

## AWS CDK

AWS 云开发工具包(CDK)是我们在这里的最新工具，于 2019 年发布。它允许开发人员使用典型的编程语言，如 Python、TypeScript、Java 和。NET 编写模板文件来管理他们的基础设施。

CDK 不是一个独立的工具，而仅仅是另一种方式，你可以用你已经熟悉的语言来利用 CloudFormation。你仍然可以享受云形成的所有好处。社区通常将它视为开发人员友好的工具，用于 AWS 中的基础设施管理。

## 地形 vs 云层 vs 自动气象站 CDK

Terraform 过去比 CloudFormation 和 CDK 有很大优势，因为它能够解决状态管理中的不一致和冲突，并更新它以自动补救任何问题。然而，CloudFormation 现在也有漂移检测，它将当前堆栈配置与模板中指定的配置进行比较，以检测任何变化或漂移。

它们与您的资源交互和管理您的资源的方式也不同。由于 CloudFormation 是 AWS 固有的，它将直接在您的基础设施上执行所有预期的操作，以达到您声明的状态。由于 Terraform 不是 AWS 本地的，并且可以跨服务和资源使用，它实际上构建了一个对 AWS 的 API 调用计划，从而创建了您的基础设施。

最近，我听说 AWS CDK 也推出了 Terraform 的 CDK，它允许开发者利用 CDK 与 Terraform 进行交互。然而，我对此并不熟悉，所以如果你想了解更多，你可以在这里找到。

## 用哪个？

请注意，以下匹配是基于我的建议，它们不一定是最佳选项，也不一定是最适合您的用例的选项。

*   简单的无服务器(或大部分)架构— AWS CloudFormation
*   仅将 AWS 用于基础架构— AWS CDK 或 CloudFormation
*   利用多个云提供商或许多不同的资源— Terraform

## 其他 IaC 工具

除了 Terraform、AWS CloudFormation 和 AWS CDK，还有很多其他的，比如 Ansible、Chef、Puppet 和 Saltstack。

最后，您选择使用哪个 IaC 工具应该基于您的需求和偏好。然而，我总是倾向于选择 Terraform，因为它给了我最大的灵活性和未来的集成。