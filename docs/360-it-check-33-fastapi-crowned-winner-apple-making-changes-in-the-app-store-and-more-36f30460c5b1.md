# 360 IT Check #33 — FastAPI 加冕冠军，苹果在 App Store 做出改变，等等！

> 原文：<https://medium.com/geekculture/360-it-check-33-fastapi-crowned-winner-apple-making-changes-in-the-app-store-and-more-36f30460c5b1?source=collection_archive---------21----------------------->

![](img/05c6338e18bf97af796bc31d8ea9fc0d.png)

## 摘要

*   苹果现在允许分享未列出的应用程序。此举可能会导致 AppStore 分崩离析；
*   FastAPI 在 StackShare 的“2021 年 100 多种开发者工具”中排名第一。堆栈共享平台根据其内部数据进行排名；
*   Cloudflare 正在向所有受到奥地利数据保护机构最近决定影响的人伸出援手，帮助他们解决 Zaraz 问题；这是所有寻找谷歌分析替代方案的人的工具。

# FastAPI 正式加冕——stack share 2021 年顶级工具的成果

StackShare 是一个平台，开发者可以在这里分享他们复杂的(绝对不是过度工程化的)技术堆栈，[分析来自他们平台的数据，找出 2021 年的顶级开发者工具。](https://stackshare.io/posts/top-developer-tools-2021)新工具类别的获胜者是… FastAPI。尽管这个解决方案已经有 3 年的历史了，但它还是在去年找到了自己的平台。它甚至已经超过了 GitHub Copilot，这是一个令人印象深刻的壮举。

当我们问及 FastAPI 的创造者[sebastián ramírez montao](https://www.linkedin.com/in/tiangolo/)时，他没有对这项成就发表任何评论，但补充道

> *我从没想过，但我很高兴它对这么多人有用！🎉*

Python 库[的创建者在](https://forethought.ai/press/forethought-hires-creator-of-fastapi-sebastian-ramirez/)工作的公司 [Forethought](https://forethought.ai/press/forethought-hires-creator-of-fastapi-sebastian-ramirez/) 的首席技术官&联合创始人‍ [萨米·戈什](https://www.linkedin.com/in/samighoche?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAA-GcSQByfWCV89s4DmVmT6Xtuh_uuFh0cU&lipi=urn%3Ali%3Apage%3Ad_flagship3_detail_base%3BGLhr98tpTQqaMubopYlSww%3D%3D&licu=urn%3Ali%3Acontrol%3Ad_flagship3_detail_base-actor_container&lici=NrR1NTY%2BQ82Vq2v1JQ50Uw%3D%3D)对这一成就感到自豪，他在 LinkedIn 上公开祝贺塞巴斯蒂安。

[](https://www.linkedin.com/feed/update/urn:li:activity:6895605774416969728/) [## LinkedIn 上的 Sami gho che:FastAPI 在 StackShare 的 100 大开发工具中遥遥领先

### FastAPI 在 StackShare 的 100 大开发工具中遥遥领先！！很难夸大这是多么不可思议的一个…

www.linkedin.com](https://www.linkedin.com/feed/update/urn:li:activity:6895605774416969728/) 

**底线**

FastAPI 是一个击败了被炒作的 GitHub Copilot(！)，混音(！)，vscode.dev(！).JetBrains Qodana，和 [Tabnine](https://www.itmagination.com/blog/rise-ai-code-autocompletion-engines-github-copilot-tabnine-kite) (!).2021 年的获胜者使 Python 生态系统更加灵活，为社区提供了一个创建 API 的工具。一些没有听说过 FastAPI 的人会问“如果我有 Flask/Bottle，为什么我还需要另一个框架。”答案很简单。新的解决方案速度更快，包括更多现成的东西，同时仍然很精简。

这里还有另一个故事。Python 的发展，有时令人畏惧(大 Python 2 迁移)，带来了一些变化，为开发人员创建和共享带有类型建议的代码打开了大门。这个特性极大地增加了企业程序员在越来越多的领域使用 Python 的机会。

*顺便说一句，不要忘记升级你的 Python 3.6 环境。*

# 苹果清理其应用商店

[开发者现在可以将他们的应用从苹果应用商店](https://mashable.com/article/apple-app-store-unlisted)中移除。过程很简单。首先，应用程序必须稳定(没有测试版应用程序将被接受)。然后他们可能[提交一份表格](https://developer.apple.com/contact/request/unlisted-app/)让他们的提交不被列出，然后……等待决定。

**底线**

虽然一开始人们可能会想为什么开发人员需要这个特性，但是有一些流行的用例。有一些应用程序可以引导与会者，充当日程表、地图等等。另一个用例是仅供内部使用的公司应用程序，出于显而易见的原因，公司希望将其隐藏。由于 iOS 不允许安装来自官方商店之外的应用程序，分发这种软件至少是有问题的，如果不违反条款和条件的话。

# 使用 Cloudflare 将分析数据保存在欧盟

奥地利数据保护局最近决定，根据欧盟法律，使用谷歌分析和美国公司的其他解决方案是非法的。鉴于这一事件，Cloudflare 提醒所有人，他们提供的分析解决方案符合欧盟法律(更具体地说是 GDPR)。

[cloud flare Zaraz](https://blog.cloudflare.com/keep-analytics-tracking-data-in-the-eu-cloudflare-zaraz/)收集的数据存储在欧盟，不会传输到欧盟以外。与此同时，分析解决方案将随着 2022 年推出的新功能而增长。

**底线**

2020 年 7 月对所有美国公司来说都是艰难的一个月。欧盟法院宣布 EU-美国隐私保护法案无效，该法案规定了数据在美国的传输方式。这项裁决被戏称为“施雷姆二世”。

随着最近的决定浮出水面，公司现在面临一个选择:继续使用谷歌分析并可能因此面临处罚，或者改变解决方案。Cloudflare 刚刚推出了自己的免费解决方案来帮助所有陷入困境的人。

*最初发表于*[*【https://www.itmagination.com】*](https://www.itmagination.com/blog/360deg-it-check-33-fastapi-python-apple-app-store-cloudflare-zaraz-google-analytics)*。*