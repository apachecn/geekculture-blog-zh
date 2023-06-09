# 数据工程师讨厌谷歌标签管理器的 4 个原因

> 原文：<https://medium.com/geekculture/4-reasons-why-data-engineers-hate-google-tag-manager-23bb6508d0db?source=collection_archive---------25----------------------->

![](img/29a65498255a7dd35ea522d8d7843b02.png)

GTM 是一个代码注入器。浏览器通常对代码注入器持负面看法，因为它们很容易成为黑客在网站或应用程序中植入恶意代码的薄弱环节。因此，GTM 被大多数广告拦截器和浏览器隐私工具拦截。据估计， [42.7%的互联网用户](https://backlinko.com/ad-blockers-users)使用广告拦截器，这意味着如果你使用 GTM，你可能会丢失 [8%到 25%](https://blog.atinternet.com/en/rocking-your-web-analytics-in-the-face-of-ad-blockers/) 的用户流量数据。如果您的整个 mar-tech 堆栈都是通过 GTM 交付的，并且被阻止，您将会遇到一些严重的挑战。

多年来，[谷歌标签管理器](https://marketingplatform.google.com/about/tag-manager/) (GTM)让营销人员和分析师可以轻松地在他们的网站和应用上安装和管理第三方分析和营销工具。它提供了一个集中的平台，允许非技术团队成员添加、编辑和禁用标签，而不必接触源代码。这意味着他们不必在更新时打扰您添加新标签或编辑现有标签。

但是，如您所知，将这样的权力交给非技术团队可能会适得其反。无论他们通过 GTM 注入到应用程序或网站中的是什么，都会绕过你通常的开发和 QA 流程，这可能会导致数据团队不得不清理的混乱。

因此，尽管 GTM 是一个有用的工具，但它的缺点对于现代数据驱动的公司来说很快就会显现出来。数据工程师讨厌 GTM，理由很充分。它对广告拦截器的敏感性和误用的倾向仅仅是一个开始。在这里，我们将详细说明 GTM 和数据工程师合不来的四个驱动原因，我们将提供一个更好的解决方案。

![](img/b3c940789bf71ca314bd49de379b5a61.png)

# 1.GTM 是一种容易出现安全漏洞的代码注入工具

由于 GTM 是一个代码注入工具，它不仅被广告拦截器拦截。它还会使您的站点被利用，因为它插入代码的能力可能会为黑客打开大门。如果黑客获得了您的 GTM 帐户，他们可以通过添加几行看似无害的代码在幕后进行破坏。通常，这些代码不会被注意到，因为它不会影响网站/应用程序的外观或功能。

在某些情况下，您的网站或移动应用程序可能会被用来显示不想要的广告，从而为黑客创造收入，或者他们会将访问者重定向到其他充满广告的网站。在最坏的情况下，你的网站/应用程序可能被利用来窃取 PII。Joom team 和 T2 Securi 都在他们的客户中记录了许多这样的例子。

像这样的安全漏洞会影响您向需要数据的团队交付高质量数据的能力，解决这些问题既费时又费钱。

# 2.它对数据驱动型企业所需的各种工具的支持有限

谷歌标签管理器为广告和分析网络提供[原生支持](https://support.google.com/tagmanager/answer/6106924?hl=en)。虽然它为网站提供了更多的支持选项，但它支持的移动应用工具不到 12 个。因此，当涉及到与您的整个堆栈中的工具集成时，GTM 只是做得不够。要将事件发送给工具，如 A/B 测试、培养、客户参与和支持，您必须想出变通办法来集成不受支持的工具。

例如，如果您正在使用定制代码通过 GTM 管理一个像 Mixpanel 这样的事件驱动工具，那么您可能想要使用另一个像 Intercom 这样的事件驱动工具。使用 GTM 意味着您必须再次编写定制代码，以便向 Intercom 发送相同的数据集。无论何时任何工具更新它的 API，你都必须返回去更新你的代码。

# 3.它会降低站点性能

如果管理不当，GTM 会给您的数字财产带来严重的性能负担。如果标签管理器容器随着时间的推移积累了标签，这可能会发生，这可能是通过对不同发布者或资产集成的实验实现的。

很多第三方跟踪像素的实现方向只是“确保它在每个页面上都可以触发”。因此，经验不足的 GTM 用户通常会使用默认的“所有页面-页面视图”触发器添加越来越多的标签，该触发器会在浏览器渲染过程的早期运行，同时还会加载和解析实际网站的关键资源(如 HTML、CSS、JS、图像、视频等)。).更糟糕的是，在默认情况下，标签本质上被视为独立的脚本。因此，如果有 50 个通过 GTM 管理的标签，浏览器现在试图在每次用户访问新页面时同时加载一个网页和 50 个不同的脚本。

虽然使用具有快速互联网连接的中型机器的用户可能不会经历严重的性能下降，但是使用较旧/较弱的机器或具有较慢连接的移动设备的用户会经历严重的性能下降。对于每次页面加载，这些较弱的设备将尽其 CPU 能力所能，然后将剩余的内容排队，锁定浏览器窗口，使网站无法运行，直到所有处理完成。这不仅会导致糟糕的用户体验，还会影响你的[核心网站指标](https://web.dev/vitals/)得分(尤其是第一次输入延迟)，这会无意中影响网站在谷歌上的排名。

[马克西米利安·沃纳](https://www.linkedin.com/in/maximilianwerner),[痴迷分析咨询](https://www.obsessiveanalytics.com/)的创始人，有着与 GTM 合作的丰富经验，他亲眼目睹了这一点。“我曾经有一个客户，GTM 导致网页在加载时冻结了大约 10 秒钟，因为页面加载时有 120 多个标签触发。移动设备基本上都崩溃了。”

# 4.大规模管理用户状态很困难

GTM 使用一个名为 dataLayer 的全局 JS 窗口变量来维护类似于间歇状态的东西。它只能跟踪当前页面上的变量。这意味着它不能跨同一域的页面持久化用户状态，更不用说跨域了。

例如，假设一个电子商务网站的结帐过程分布在四个页面上:购物车、个人信息、支付信息和确认。当用户在个人信息页面上输入他们的姓名和电子邮件，你使用`dataLayer.push({name: 'john doe', email: "johndoe@domain.com" })`手动或通过电子商务网站自动运行的代码存储该信息(如果他们提供与 GTM 的集成)，然后你可以在该页面上使用该信息。

然而，当用户导航到下一页时，`dataLayer`被清除，因为它只是一个窗口变量，您将不再能够访问该信息。为了将数据带到下一页，您必须自己编写定制的 JS 来持久化数据，将它存储在 cookie 或 localStorage 中，然后在下一页上读出它。这增加了更多的失败点，并阻止您充分考虑在不同阶段加入漏斗的用户。

# 谷歌标签管理器有更好的替代品

GTM 根本无法满足不断增长的数据驱动型业务的需求——尤其是当数据堆栈变得越来越复杂时。对于开启高级用例的灵活和可伸缩的解决方案，您将需要一个更健壮的工具，如用于数据集成的 RudderStack。

使用 RudderStack，您不必担心安全漏洞，因为 RudderStack 不会将代码注入到您的网站中。你也不必担心广告拦截器——RudderStack 使用一个[代理的 rudder stack 和服务器到服务器的集成](https://www.obsessiveanalytics.com/blog/making-rudder-stack-ad-blocker-proof-in-66-lines-of-code)。说到对工具的支持，RudderStack 为 200 多个工具提供了[集成](https://rudderstack.com/integration)。RudderStack 的集成范围从广告网络和企业通信到归因和营销工具，它使您能够轻松集成您已经使用的工具，并在您的堆栈增长时为您提供灵活性，以满足新的业务需求。

RudderStack 还让您的生活变得更加轻松，尤其是在不同的访问之间保持用户会话数据的完整性，因为它保存了用户状态，并且可以通过`rudderanalytics.getUserProperties()`随时访问。

简而言之，RudderStack 使您能够为每个团队提供他们需要的客户数据。使用 RudderStack 的 SDK 进行一次测试，然后自动将数据发送到您的仓库和业务工具。不再需要处理 API 变更和管道中断。你甚至可以通过 RudderStack 使用 GTM。因此，如果你想继续使用谷歌标签管理器来管理你的广告像素和其他谷歌相关的工具，你可以将其与 RudderStack 集成。

# 立即尝试更好的数据集成解决方案

测试我们的事件流、ELT 和反向 ETL 管道。使用我们的 HTTP 源在不到 5 分钟的时间内发送数据，或者在您的网站或应用程序中安装我们 12 个 SDK 中的一个。[上手](https://app.rudderlabs.com/signup?type=freetrial)。

*本博客最初发布于:* [*https://rudder stack . com/blog/4-reasons-why-data-engineers-hate-Google-tag-manager/*](https://rudderstack.com/blog/4-reasons-why-data-engineers-hate-google-tag-manager/)