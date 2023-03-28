# 几乎一半的假肢使用者放弃了他们的医疗设备。机器学习可以改变这一点

> 原文：<https://medium.com/geekculture/almost-half-of-all-prosthetic-users-give-up-on-their-medical-devices-ml-can-change-that-ea53ecae0be5?source=collection_archive---------11----------------------->

![](img/436541f17223fb7f71a6311e55a5c93c.png)

假肢的概念已经存在了几千年，已知最早的假肢——一个大脚趾——发现于埃及，可追溯到公元前 1000 年。

最近的历史见证了不同假肢的快速发展，包括配备了几十个传感器的[机器人动力假肢](https://www.sralab.org/research/labs/neural-engineering/projects/advanced-control-systems-powered-prosthetic-legs)，这些传感器收集关于位置、角度和施加的力的实时数据。

但是许多假肢都有一个大问题，这个问题可能从第一个大脚趾是为一位埃及贵妇设计的时候就存在了:[人们不喜欢它们](https://www.inputmag.com/culture/cyborg-chic-bionic-prosthetic-arm-sucks)。

事实上，即使在技术先进的假肢时代，排斥率——或者说病人停止使用假肢的比率——也接近 50%。

但是当病人使用人体动力或电动假肢时，排斥率下降了将近一半。许多研究人员现在将机器学习(ML)视为这些非被动假肢的完美补充。

## 机器学习对更好的假肢的承诺

修复术肯定比几十年前更精确、更强大，也更先进。所有这些嵌入现代假肢的实时传感器都可以捕捉大量数据。

然而，直到现在，他们还不能对假肢佩戴者的生活质量产生巨大的影响，大多数截肢者只有非常基本的控制能力。许多假肢使用起来不直观，而且携带起来很重。

类似地，大多数传统的假体装置具有[有限的](https://www.media.mit.edu/publications/design-and-control-of-a-two-degree-of-freedom-powered-ankle-foot-prosthesis/)自由度(DOF)运动和范围。这意味着这些肢体中的大多数只执行一种或两种功能，而且通常不是很好。

人工智能和科学记者 Devin Coldewey 在 [*TechCrunch*](https://techcrunch.com/2019/09/13/this-prosthetic-arm-combines-manual-control-with-machine-learning/) 中写道:“许多原本控制手指的肌肉和肌腱都消失了，随之而来的是准确感知用户想要如何弯曲或伸展他们的人造手指的能力。”。“如果用户所能做的只是发出一个普通的‘握紧’或‘松开’信号，那就失去了一只手真正擅长的大量功能。”

然而，这种情况可能会改变，因为下一代多自由度假肢与 ML 模型配对，后者可以自动检测和学习抓握和其他详细运动的细微点。

当假肢使用者尝试各种运动时，这些模型通过观察肌肉信号和其他刺激来学习。有了这些信息，假肢——以及为它们提供动力的人工智能模型——学会自动执行某些平凡而重要的动作，类似于实际手或腿的肌肉记忆，确保更有效和更少的一维抓握和运动。

正如阿尔伯塔大学[的研究人员解释的那样](https://sites.ualberta.ca/~pilarski/docs/theses/Vasan_Gautham_201709_MSc.pdf)，这些技术可以帮助用户和他们的智能设备分担“控制负担”，假肢设备本质上为用户“填补空白”。

## 通过机器学习改进假肢

研究人员最近在使用强化学习和其他 ML 技术改进假肢方面取得了显著进展，至少在受控实验中是如此。

[2017 年](https://pubmed.ncbi.nlm.nih.gov/28814025/)，Vasan 等人(上文提到的 A 研究人员的 U)演示了一种用于假肢训练的 actor-critic 强化学习方法。该技术允许用户使用他们未截肢的手臂通过协调运动和抓握来训练 3 自由度假肢。

Vasan 等人的实验表明，ML-powered 假肢可以只根据机器人手臂收集的数据和用户肘部以上的肌电信号进行训练，以执行“同步手势和运动”。

“这些初步结果，”作者写道，“还表明我们的方法可能会以直接的方式扩展到下一代具有精确手指和手腕控制的假肢，这样，这些设备可能有一天会允许用户进行流畅和直观的运动，如弹钢琴、接球和舒适地握手。”

两年后，[2019 年](https://news.ncsu.edu/2019/01/reinforcement-learning-expedites-tuning-of-robotic-prosthetics/)，北卡罗来纳大学、北卡罗来纳州立大学和亚利桑那州立大学的研究人员开发了一种以强化学习为基础的技术，来调整机器人假肢膝盖。

该系统从设备中收集数据，同时将患者的步态与典型的健康步态进行实时比较，在分析数据的同时，几乎在整个步态周期中即时调整 12 个控制参数。

该技术允许患者在 10 分钟内在水平面上行走，这与机器人膝盖支架改造通常需要的半天人工训练相比，是一个巨大的变化。

最近的研究收集了更有希望的结果。Swami 等人的 [2021 年论文](https://www.nature.com/articles/s41598-021-94449-1)展示了如何使用从 10 个使用手腕支架执行日常生活(ADL)任务的个人的活动中收集的数据来训练 2 自由度假肢。

神经网络首先对每个动作进行分类，用随机森林回归计算动作速度。这项研究的分类取得了 99%的 F-1 分数，而根据皮尔逊相关测量，回归得分为+0.98(1 是完全正相关)。

## CapeStart:您的医疗机器学习伙伴

CapeStart 在帮助医疗研究人员将现成的和定制的机器学习算法和模型应用于系统综述和真实世界的临床应用方面拥有多年的经验。[今天就联系我们](https://www.capestart.com/about-us/capestart-is-your-end-to-end-data-annotation-machine-learning-and-software-development-partner/)安排一次我们专家的简短拜访，了解我们能为您的下一个项目做些什么。