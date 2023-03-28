# 360 IT 检查#30 — Node.js 安全漏洞、Docker 单点登录等等！

> 原文：<https://medium.com/geekculture/360-it-check-30-node-js-security-vulnerabilities-docker-single-sign-on-and-more-2256ad02d3c6?source=collection_archive---------19----------------------->

![](img/f5d80c8aef15263fba0a39ff99d66a69.png)

# 四个 Node.js 版本具有相同的四个漏洞

1 月 10 日，Node.js 背后的团队[公布了影响这个 JavaScript 引擎四个版本的](https://nodejs.org/en/blog/vulnerability/jan-2022-security-releases/) 4 个安全漏洞。尽管没有一个是高严重性的，但重要的是要注意所有维护的版本都需要升级。这有一点问题，因为并不总是能够顺利升级他们的生产环境。

**底线**

如果在您的计算机或服务器上运行的是 Node.js 的旧版本，那么为了安全起见，有必要对其进行升级。找到你的操作系统的下载链接，或者安装说明[这里](https://nodejs.org/en/download/)。

报告的链接:

*   https://cve.mitre.org/cgi-bin/cvename.cgi?‍name=CVE-2021-44533
*   https://cve.mitre.org/cgi-bin/cvename.cgi?‍[name=CVE-2021-44532](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44532)
*   https://cve.mitre.org/cgi-bin/cvename.cgi?‍[name=CVE-2021-44531](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44531)
*   [https://cve.mitre.org/cgi-bin/cvename.cgi?](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44531)‍[name=CVE-2022-21824](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-21824)

# 一家以拯救互联网为使命的公司

Astro.js 过去纯粹是个人的运动，他们相信一个更健康的互联网，没有大量的 JavaScript 捆绑包。[自上周](https://astro.build/blog/the-astro-technology-company/)以来，“太空技术公司”现在正牵头努力，并接管了回购这一前景看好的工具的所有权。

**底线**

正如我们所知，网络的状态并不理想。网站向用户提供大量的 JavaScript，因为这对开发团队来说是件容易的事情。即使搜索引擎，如谷歌，惩罚大量下载，它也不能阻止企业向用户发送不必要的代码。Astro.js 的出现，消除了所有不必要的膨胀。一个人甚至不需要学习 React、Vue 或 Svelte——你可以使用 Astro 组件，这是类固醇上的 HTML。一点小旁注:Astro 不支持 Angular。

# Red Hat 上的 Node.js

一月是一个特殊的月份，在这里我们仍然在反思去年发生的事情。Red Hat 也这么认为，因为他们上周[分享了他们对 Node.js 使用的总结](https://developers.redhat.com/articles/2022/01/10/nodejs-red-hat-2021-year-review#things_we_shipped)。

有几点值得提出来。

**底线**

积极参与 Node.js 社区生活是很重要的，因为 Red Hat 有相当多的资源致力于这一重要工具的开发。许多大公司已经参与了这个 JavaScript 工具的开发。保持均衡很重要，没有哪家公司有明显的优势。

该公司还帮助开发商进行专业开发，并通过他们的参考指南吸引新的开发商。

# Docker 单点登录

客户大量要求 Docker SSO。例如，Docker Hub、[背后的公司听取了](https://www.docker.com/blog/introducing-sso-for-docker-business/)的意见，并为他们的“业务层”客户介绍了该功能。

由于这一功能，公司可以轻松地大规模接纳新用户，并管理他们的账户。要求是使用 SAML，和 Azure 单一目录身份提供者。

**底线**

许多大公司可能会对这项功能感到满意。使该功能广泛可用会使大规模管理变得更加容易。另一个赢家是微软，因为这是使用他们云服务的一个要求。

***【360 IT Check****是一份周刊，在这里我们为您带来世界上最新最棒的技术。我们涵盖了新兴技术&框架、创新创业公司的新闻以及其他直接或间接影响技术世界的话题。*

*喜欢你正在阅读的东西吗？请务必订阅我们的* [*每周简讯*](https://www.itmagination.com/newsletters/360-it-check) *！*

*原载于*[*https://www.itmagination.com*](https://www.itmagination.com/blog/360deg-it-check-30-node-js-security-astro-red-hat-sso-docker)*。*