# 利用机器学习拯救生命(并且更高效)的住院和门诊病人分类

> 原文：<https://medium.com/geekculture/utilizing-machine-learning-for-life-saving-and-more-efficient-inpatient-and-outpatient-triage-d684a5275632?source=collection_archive---------17----------------------->

![](img/da7fa56b70fbefed2aa90e4eca30ea60.png)

分诊是现代医学的一个基础部分，可以追溯到神圣罗马帝国时期。在拿破仑战争期间，法国军医多米尼克·让·拉雷开发了现代的伤检分类技术，并在处理战场伤害时结合了新发现的对卫生的重视。

几百年后，分流现在是现代急诊部门的一项重要职能，由于患者数量增加、人口快速老龄化、长期员工短缺以及疫情的兴衰，急诊部门经常达到容量极限。

然而，自 15 世纪以来，分诊的一个要素一直没有改变，那就是分诊一直完全由人类进行。至少，直到现在。

## 机器学习如何帮助改善分诊(尤其是门诊病人)

在远程医疗或远程健康监控系统(RHMS)环境中，当患者不在医院接受观察时，接听分流电话对人类来说尤其具有挑战性。

但是远程医疗和 RMHS 系统是医疗保健行业解决医院过度拥挤的[关键工具之一。](https://www.sciencedaily.com/releases/2020/11/201130131443.htm)

这就是为什么研究人员已经开始在医院和远程医疗应用中应用机器学习(ML)工具来帮助扩展分诊活动。这些模型还可以更好地支持医疗保健工作者，他们在执行分流时通常必须依靠自己的知识和经验。

自 20 世纪 80 年代以来，医疗保健提供商一直使用其他临床决策支持系统(CDSS):2013 年的统计数据显示，[41%能够访问电子病历的美国医院使用 CDSS。然而，这些系统通常只不过是静态的在线数据库，无法对特定的患者问题和病史做出智能反应。](https://www.nature.com/articles/s41746-020-0221-y)

相反，ML 模型从真实世界的数据集和经验中学习，并且[使用算法](https://towardsdatascience.com/triage-to-ai-a-machine-learning-approach-to-hospital-admissions-classification-7d3a8d5df631)来“为我们区分患者的优先次序，而不是花费时间来生成甚至可能在提供商之间不一致的评级。”

除了提高分诊效率和准确性，ML 模型还有可能通过促进对门诊患者更准确的评估来节省医疗保健提供商、支付者和消费者的大量资源。在美国，住院三天的平均费用约为 3 万美元。

## 机器学习和病人评估:研究

当然，当病人的生命悬而未决时，这不仅仅是费用的问题。事实上，一项基于英国[的研究](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5437335/)显示，超过 70%的病例因眼科疾病的治疗延误导致永久性眼部损伤，这说明了快速准确的分类的必要性。

虽然仍处于相对初级阶段，但大量研究已经检验了 ML 模型在住院和门诊分诊中的功效。以下是最近一些最引人注目的研究。

**李等(2021)** :研究人员[考察了](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8507354/)在香港眼科专科门诊(SOPC)评估患者时，人工智能辅助分诊如何帮助减少从业者的工作量。

在其他发现中，该研究指出，人在回路中的机器学习模型(结合了人类研究人员和 ML 模型)比只有人工智能或只有专家的系统表现更好(AUC 得分[分别为. 84 和. 763 和. 685)。](https://stats.stackexchange.com/questions/132777/what-does-auc-stand-for-and-what-is-it)

**谢等人(2021)** :在[这个案例](https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2783549)中，研究人员对比其他临床评分，观察了基于 ML 的分诊工具在估计住院患者死亡率方面的有效性。

研究人员开发了基于 ML 的紧急风险预测评分(SERP)工具，并分析了 2009 年至 2016 年新加坡的 ed 患者。该工具由同一批研究人员开发的基于 ML 的评分框架 [AutoScore](https://medinform.jmir.org/2020/10/e21798/) 支撑。

[根据](https://healthitanalytics.com/news/machine-learning-triage-tool-better-predicts-mortality-in-ed)到*医疗 IT 分析*的说法，与非 ML 分诊系统相比，该工具提供了更“准确的患者死亡风险评估”,如患者急性分类量表、修改后的预警评分、国家预警评分、心脏骤停风险分诊、快速急性生理学评分和快速急诊医学评分。

“SERP 比现有的分流分数有更好的预测率，并在 ed 中保持了易于实施和易于确定的特点，”*医疗 IT 分析*写道。

**Hadi 等人(2019)** :这项来自利兹大学的[研究](https://www.researchgate.net/publication/331768609_Using_Machine_Learning_and_Big_Data_Analytics_to_Prioritize_Outpatients_in_HetNets)使用 ML 模型检查了门诊患者的优先顺序，首先使用原生贝叶斯分类器分析门诊病历和医疗设备数据。然后，ML 模型吸收这些数据来预测威胁生命状况的可能性。

特别是，本研究考察了两种上行链路技术:加权和速率最大化(WSRMax)和比例公平(PF)。“使用这些方法，”作者写道，“我们在为医疗物联网传感器提供可靠连接方面展示了拟议系统的效用，使 OPs 能够保持连接的质量和速度。”

**Miles et al. (2020):** 该[系统综述](https://diagnprognres.biomedcentral.com/articles/10.1186/s41512-020-00084-1)使用 Medline、CINAHL、PubMed 和其他来源的医学文献，分析了 ML 模型在所有入院患者(不仅仅是门诊患者)急诊分诊中的准确性。

研究人员使用测量疗效的 C 统计方法分析了 92 个模型，神经网络和基于树的方法记录了需要住院治疗的患者的中值分数. 81(高于. 8 的值通常表明[强模型](https://www.statisticshowto.com/c-statistic/#:~:text=Values%20over%200.7%20indicate%20a,and%20those%20who%20will%20not.))。对于那些需要重症监护的患者，神经网络的 C-统计中值为 0.89。

虽然神经网络似乎总体得分较高，但研究人员指出，“逻辑回归得出的模型在报告模型性能方面更透明。”

**Fernandes 等人(2020)** :最后，这个[系统综述](https://pubmed.ncbi.nlm.nih.gov/31980099/)检查了来自 ScienceDirect、IEEE Xplore、谷歌学术、Springer、MedlinePlus 和 Web of Knowledge 的文献，以评估基于 ML 的工具与传统的分类方法。

研究人员发现，逻辑回归是模型开发最常用的方法。大多数应用侧重于改善患者优先级、预测重症护理需求和医院/ICU 入院、住院时间和死亡率。

“在 ed 中验证 CDSS 的论文中，作者发现卫生专业人员的决策有所改善，从而导致更好的临床管理和患者的结果，”作者写道。然而，他们也注意到，半数以上的审查研究没有包括实施阶段，因此难以评估其有效性。

## CapeStart:医学机器学习专家

CapeStart 的机器学习工程师、医疗主题专家和数据科学家每天都与领先的医疗保健和研究机构合作，通过先进的机器学习来改善医疗成果。我们的现成和定制 ML 模型、训练数据集和数据注释服务是为提高复杂医学研究和临床应用的效率和功效而量身定制的。