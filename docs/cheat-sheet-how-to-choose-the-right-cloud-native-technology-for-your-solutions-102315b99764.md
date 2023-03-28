# 备忘单:如何为您的解决方案选择合适的云原生技术

> 原文：<https://medium.com/geekculture/cheat-sheet-how-to-choose-the-right-cloud-native-technology-for-your-solutions-102315b99764?source=collection_archive---------8----------------------->

![](img/b1b2f9e83033998652262baac9bf4d12.png)

Photo by [Kvalifik](https://unsplash.com/@kvalifik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着云的爆炸式增长及其优势，它已经成为大多数软件开发的事实上的选择。无论是新开发还是大修，将现有开发迁移到云已经成为最有吸引力的选择。在本文中，让我们看看如何为您的解决方案选择合适的云原生技术堆栈。

# 什么是云原生技术？

云原生技术是指旨在利用云计算模型提供的灵活性、可伸缩性和弹性的技术。云原生技术[通常支持面向微服务的](https://microtica.com/blog/everything-about-microservices/?utm_source=medium&utm_medium=referral_link&utm_campaign=cloud_native&utm_content=geekculture)、容器化的和动态编排的应用，API 充当这些组件之间的通信机制。

# 云原生技术与传统技术有何不同？

云原生技术和传统技术的主要区别在于它们面向的应用程序类型。传统技术主要适用于内部部署的桌面或基于服务器的应用程序，这些应用程序本质上更加单一。另一方面，[云原生技术](https://microtica.com/blog/a-comprehensive-introduction-to-cloud-native-devops/?utm_source=medium&utm_medium=referral_link&utm_campaign=cloud_native&utm_content=geekculture)更倾向于与分布在自然界的基于网络的应用程序协调一致。

这种分布式特性允许云原生技术满足分布在全球的广泛用户的需求，而不会遇到性能瓶颈。云原生技术还允许用户轻松利用许多不同的服务、提供商和平台，然后将它们的优势整合到应用程序中。

# 云原生的优势

*   基于消费的定价模式允许用户[经济高效地管理他们的资源](https://microtica.com/blog/reduce-aws-costs-on-non-production-environments/?utm_source=medium&utm_medium=referral_link&utm_campaign=cloud_native&utm_content=geekculture)。他们可以避免过度配置，只需为应用消耗的资源付费。
*   近乎无限的可扩展性使应用程序能够根据用户需求进行扩展，从根本上消除了性能瓶颈。在基于微服务的架构中，这种可扩展性与每个服务的独立扩展相关。
*   增加了可移植性，因为应用程序可以很容易地迁移到不同的提供商，而无需经历巨大的变化。容器化的应用程序尤其如此，同时也避免了供应商锁定。
*   非常适合自动化。从开发到部署和管理的大多数方面都可以轻松实现自动化，从而带来相对更轻松的管理体验。
*   通过创建隔离的服务，使用容器和微服务等云原生技术可以实现更好的安全态势。这种隔离最大限度地降低了对整个解决方案的影响，即使单个组件受损也是如此。

# 云原生开发的最佳选择是什么？

技术的选择完全取决于用户的确切需求。然而，我们可以将通用用例的可用技术分类如下。

*   集装箱化——LXD 码头，集装箱
*   编排— Kubernetes、Rancher、Openshift
*   服务网络— Istio、Lankerd、Cosul
*   CI/CD — Jenkins、GitLab、GitHub Actions、Microtica、Flux、ArgoCD
*   基础设施(IaC) — Terraform、Plumi、Ansible

云原生应用不能依赖单一技术。它总是针对特定用例的技术组合。

# 选择云原生技术时，哪些参数很重要？

云服务为普通消费者带来了数据中心功能，而没有与维护数据中心相关的管理责任或成本。即使对于像 Kubernetes 这样的服务，也有一些托管解决方案可以减轻这些服务的日常管理需求。然而，在选择合适的技术时，需要考虑一些关键参数。

首先是**类型的解决方案及其架构**。根据特定的使用情况，技术堆栈可能会有很大的不同。例如，像 AWS Elasticbeanstalk 这样的服务可以简化一个简单的 web 应用程序，而 [Kubernetes cluste](https://microtica.com/blog/kubernetes-for-app-developers/?utm_source=medium&utm_medium=referral_link&utm_campaign=cloud_native&utm_content=geekculture) r 中的容器将是更复杂的解决方案的理想选择，因为它们具有更强的控制能力。

接下来是**技术的易用性**。即使技术非常适合需求，如果它有一个更大或者更复杂的学习曲线，它也会影响解决方案的交付。在某些情况下，利用多种相对简单的技术可能会提供更好的解决方案。

**安全性和合规性**也是云原生技术选择流程的关键参数。所选择的技术应该符合[合规性要求，例如数据冗余](https://www.techtarget.com/searchstorage/definition/redundant)、访问控制和加密，然后才能向前发展。此外，在没有现成的安全服务或支持来减轻安全威胁的情况下，不应该使用未经验证或极易受到攻击的技术来增强您的解决方案。拥有大型社区或商业支持的成熟技术将更适合安全和健壮的开发。即使已经检测到漏洞，具有更强支持结构的技术也有更大的机会快速获得修补漏洞的补丁。

最后一个参数是**维护工作量和成本**。成本考虑不仅仅限于技术本身，还包括辅助因素，如人力资源和学习和适应技术的时间限制。因此，如果选定的技术不能在给定的成本限制内进行管理，就不应该选择它。维护包括监控应用程序并管理其日常维护活动。当涉及到维护时，应该考虑技术暴露于监控方面的能力，如指标、日志及其内置的监控工具。

# 如何选择？

现在，我们已经很好地了解了云原生技术，以及在选择合适的技术堆栈时应该考虑的因素。因此，让我们来看看我们如何启动您的云原生应用程序的各个方面。

# 选择核心应用技术

主要任务是深入了解解决方案的确切需求。应用程序的主要目标是什么？它试图解决什么问题？目标受众是什么？等等。通过理解和定义需求，您可以专注于定义应用程序架构。

系统架构将决定可以使用哪种技术堆栈。像容器这样的技术几乎可以用于任何类型的开发。凭借其自包含和可移植的特性，您可以相对容易地开发和部署您的应用程序。使用 AWS ECS 等完全托管的服务，可以轻松部署在单个容器中运行的简单应用程序。然而，随着复杂性的增长，您将需要越来越多的容器来支持不同的组件，甚至是不同地区的多个部署。因此需要额外的功能，如负载平衡、服务发现、安全性、错误处理、部署管理等。这就是 Kubernetes 等服务通过提供完整的容器编排引擎来管理容器生命周期的原因。

更进一步，如果您的应用程序有多个独立的组件执行一个专用的业务功能，您应该选择基于微服务的架构。基于微服务的架构可以独立扩展，是一种容错的面向服务的结构。

随着这些服务复杂性的增加，在一个集群或多个集群内的服务之间路由流量将变得越来越复杂。然后，像 Istio 这样的服务网格解决方案适合提供专用的基础架构层，以促进安全的服务对服务通信、负载平衡、故障转移、可插拔策略等。需要提高环境的可观察性，以适当满足应用程序不断增长的管理需求。即使您的应用程序不需要所有这些功能，创建一个能够集成不同技术的架构从长远来看也是有益的。

因为微服务和服务网格默认会导致更安全的应用架构。采用这样的结构在可伸缩性、可用性、安全性和易管理性等多个方面都有好处。

# 将基础设施实现为代码或手动管理

一旦你理清了架构，下一个目标就是找到支持的基础设施服务。即使对于托管服务，也有供应或配置它们的要求。因此，基础设施管理技术也应该包含在您的产品组合中。这导致了代码为的 [IaC 或基础设施，这对于实现 GitOps 之类的实践是必不可少的，并且对于以标准化方式管理大规模基础设施配置是非常宝贵的。但是 IaC 不应该包含在您的堆栈中，除非它能带来实实在在的好处，如节省时间、环境标准化等……因为这可能会给较小的开发带来不必要的管理开销。](https://microtica.com/blog/infrastructure-as-code-from-the-beginning-to-gitops/?utm_source=medium&utm_medium=referral_link&utm_campaign=cloud_native&utm_content=geekculture)

IaC 也有不同的风格，云提供商提供自己的工具，第三方解决方案提供平台无关的工具，如 Terraform 或 Pulumi。虽然你可以考虑一个特定于平台的工具，比如 AWS CloudFormation 或者 Azure Resource Templates，但是它会导致厂商锁定。因此，使用可以跨不同平台和服务使用的 IaC 工具总是明智的。

# 促进开发环境

一旦您决定了支持您的应用程序的技术，接下来您应该考虑使用什么工具来管理您的开发环境，以及在交付过程中应该使用什么实践。像 DevOps 和 [GitOps](https://microtica.com/blog/gitops-devops-for-infrastructure-automation/?utm_source=medium&utm_medium=referral_link&utm_campaign=cloud_native&utm_content=geekculture) 这样的实践将需要更加重视持续集成，CI/CD 工具将成为交付渠道的支柱。

# 连续监视

难题的最后一部分是应用程序维护。应用程序进入生产环境后，应该对其进行监控，以识别从安全漏洞到性能问题等各种问题。因此，您必须监控应用程序和所有支持基础架构的日志和指标。因此，提供本机监控功能的工具将是更好的选择。Kubernetes、Istio 就是这类工具的例子，它们提供了增强的环境可观察性，并为环境中的所有实体提供了监控能力。

即使上述所有因素都达成一致，用户也必须能够灵活地切换技术，因为他们可能会面临意想不到的复杂情况，如有限的可扩展性、功能贬值、会限制技术堆栈可用性的成本约束。因此，在完全投入使用之前，对您选择的技术进行试运行总是明智的。

# 结论

选择正确的云原生技术可以决定一项开发的成败。该备忘单涵盖了开发人员在选择云原生技术堆栈时应该考虑的所有主要方面。我希望它能帮助您为下一次云原生开发选择正确的技术。