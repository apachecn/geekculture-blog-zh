# FAANG 系统设计面试简介

> 原文：<https://medium.com/geekculture/introduction-to-system-design-interviews-at-faang-27b39a7b715f?source=collection_archive---------3----------------------->

当了四年的软件开发人员后，当我开始找新工作时，我在云环境、组件或规模设计方面几乎没有任何经验。
然而，由于深入的研究，我在系统设计面试中表现出色，在本指南中，我以问答的形式分享了关于这些面试的见解—&为什么这些面试很难，他们试图测试什么，**回答系统设计问题的模板**，等等。

## 系统设计面试有什么难点？

这些采访没有明确的格式，所以看起来。它从一个任意的问题、一段对话和一些图画开始。
事实上，有一个很好的结构和步骤可以遵循，我稍后会详细介绍。

但的确，与编程问题不同，系统设计中没有关于成功的明确定义；“一个好的答案”是一种流动的，很难衡量。

你的经历和性格创造了一个独特的答案和解决方案，可能不像别人；只要你把基本的东西做对了，剩下的答案就取决于解释了，没有对错之分。

但即使没有一个明确的答案，如果我们明白面试测试的是什么品质，我们就可以提出来。

## 系统设计面试试图测试什么？

不同的测试人员可能有其他的目标，但是最重要的是，目标是检查你的技术领导者素质。

比如你考虑一个问题的哪些方面，能不能建议几个解决方案，说出区别——利弊，挑一个？这是把一个大问题分解成更多小问题

## 如何在系统设计面试中失败？

如果你不明白什么是测试(看完这个帖子应该不会发生)，你就可能失败。或者毫无准备，不发起讨论，保持沉默，让面试官一个接一个地问你设计的各个方面。

你也可以关注这次面试没有测试的错误的东西——你的编码能力和算法知识。

## 怎么过？

使用下面提到的结构，然后按照步骤解决设计问题。
针对你思考的每个问题，找出两个或两个以上的解决方案，列出取舍，选择单一答案。

善于沟通——你应该说更多的话，听听面试官要说什么；他们可能想引导你谈论某些方面，或者阻止你选择错误的解决方案——让他们帮助你。

## 如何在系统设计面试中脱颖而出？

通过提及边缘案例、现有的现成解决方案或该问题的历史来展示广阔的视野和经验，但不要细说。

**走技术交流的黄金路线**——从头到尾引导面试，清楚地描述问题各个方面的解决方案，同时注意面试官的反馈。

通过展示所有的陷阱来跟进您的设计——提及瓶颈、安全方面、随着时间的推移将出现的技术债务，以及当前解决方案的规模限制。
讨论你的缺点表明你对自己的想法有信心，即使这些想法并不完美，也表明你有责任和不自私地找出你建议的缺点。

# 你的行业经验为面试树立了标杆

初级工程师不需要设计系统，因为这种技能是通过他们尚不具备的行业经验建立起来的。
你可以查看[这个帖子](https://medium.com/r?url=https%3A%2F%2Flevelup.gitconnected.com%2Fhow-to-improve-software-architecture-skills-daily-6f362d4e6493)来提高你的软件架构技能，不管面试。

高年级学生被要求很高的标准，并被期望对软件架构有广泛的理解和对问题的概述。有时候一个面试流程会有两个系统设计面试。

中级工程师可以参加系统设计面试，但期望比高级工程师低。

## 这次采访的含义是什么？

这个面试可能会影响你进入公司时的水平。虽然大多数人不会在面试中失败，但你的表现对薪水和你在这家公司的起点有着重大影响。

## 系统设计面试的范围是什么？

深度和广度由面试官决定；他们可能会也可能不会钻研现有技术，比如命名和选择数据库或 AWS 服务。相反，他们可能专注于 API 设计，要求你计算用户负载或处理概念规模的挑战。

向招聘人员询问更多信息，为你的成功创造更好的条件；是否期望你对具体技术有深刻的理解？他们测试高层次的问题解决或技术选择吗？你会用什么绘图工具？

提问显示了你的奉献精神和投入努力通过测试并留下好印象的意愿。

# 面试结构——如何回答一个问题？

在你的回答中，试着联系这些方面(按此顺序):

1.  明确需求
    -特性——定义来自用户的[功能需求](https://www.youtube.com/watch?v=xRdjEd-VQTU&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=28)。
    -工程限制-询问规模、吞吐量、安全性、隐私、网络和移动支持。
2.  定义系统 APIs 针对超出范围的部分。
    它可以面向前端，也可以来自第三方服务。
3.  估算并计算
    -规模-系统将支持多少[个并发用户](https://dev.to/ievolved/how-i-calculate-capacity-for-systems-design-3477)。
    -存储-我们需要多少空间来存储数据。
    -网络带宽
4.  设计数据模型
    -识别系统中的[实体](https://www.youtube.com/watch?v=aSvOThsVe5w&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=2)和交互。
    -考虑数据管理、存储、传输、加密。
5.  高级设计
    绘制 5-6 个组件，并展示系统的端到端流程[。
    这是您认为的“经典系统设计”部分，请注意我们在完成这一部分之前采取了多少步骤。](https://www.youtube.com/watch?v=YBRGp_CNzXw&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=5)
6.  细节设计和权衡
    一旦你对全局有了清晰的了解，也就是深入思考 2-3 个问题/组成部分的地方，面试官可能会引导你到感兴趣的部分，并讨论不同的方法、利弊。
    此外，改进一个简单系统的不同方法，比如[缓存](https://www.youtube.com/watch?v=Ez1GK2imrsY&list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a&index=9)，优化读或写，等等。在第一步中，你定义了用户体验应该是什么——在这一步中展示如何实现它。
7.  找到故障
    对你的设计进行了概述，找到&勇敢地讨论尽可能多的问题，例如瓶颈、同步问题、单点故障，并提出一些处理它们的方法。
    不要忘记解决系统的监控问题——您可能需要不同的日志和警报。

更多的资源是这些 [5 个小技巧](https://www.youtube.com/watch?v=CtmBGH8MkX4)、 [1 小时一集](https://www.youtube.com/watch?v=ZgdS0EUmn70)和一个[全程](https://www.educative.io/courses/grokking-the-system-design-interview)。

# 总结

我希望你对 SDI 有一个更全面的了解，他们测试什么以及如何通过测试。
相关岗位— [忙碌面试者冲刺计划](https://towardsdatascience.com/sprint-plan-for-the-busy-interviewee-16fd0893623d)和[忙碌面试者技术准备](https://towardsdatascience.com/technical-preparation-for-the-busy-interviewee-9082830550e5)。

![](img/dbf34f952e89b26ad184f33708da2b5c.png)

Photo by [Jan Tinneberg](https://unsplash.com/@craft_ear?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ways?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)