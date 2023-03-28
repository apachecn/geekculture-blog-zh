# 自动化药物警戒:检测科学文献中药物不良反应的 ML 和 NLP

> 原文：<https://medium.com/geekculture/automating-pharmacovigilance-ml-and-nlp-for-detecting-adverse-drug-reactions-in-scientific-44feec8b779f?source=collection_archive---------25----------------------->

没有人喜欢药物不良反应(ADRs)——制药公司不喜欢，监管机构不喜欢，当然病人也不喜欢。

这就是为什么持续监测药物不良反应——一个被称为药物警戒(PV)的过程，或监测药品的安全性和风险效益概况——是全球制药公司的核心责任。

然而，尽管如此警惕，药品不良反应仍然是一个日益严重的问题，占欧盟急诊室就诊的近 30%。结合其他挑战，如全球人口老龄化、更广泛的慢性疾病以及市场上日益复杂的一系列药品，显然通过 PV 识别和跟踪 ADR 比以往任何时候都更加重要。

通过机器学习和自然语言处理(NLP)实现 PV 的智能自动化可以提高 PV 和 ADR 监测的有效性。但是首先有必要回顾一下更传统的 PV 方法——并检查为什么它们变得有些过时了。

![](img/b8aa8aea68110285a675cb256d5ab3a0.png)

## 传统药物警戒的问题是

PV 发生在药品的整个生命周期中，从临床试验到上市后监测。该行业传统的 PV 方法植根于[自发报告系统](https://globalpharmacovigilance.tghn.org/articles/spontaneous-reporting/#:~:text=Spontaneous%20reporting%20has%20been%20defined%20as%3A&text=Spontaneous%20reporting%20is%20by%20nature,local%20or%20national%20pharmacovigilance%20centre.) (SRSs)，其中包含由医疗专业人员或消费者提交给监管机构或制药公司的 ADR 报告。

很难低估 SRSs 对药物警戒的重要性。事实上，世界卫生组织[说](https://www.who.int/medicines/areas/quality_safety/safety_efficacy/PV_Minimum_Requirements_2010_2.pdf?ua=1)国家 SRS 系统是有效的国家光伏战略的五个最低要求之一(其他要求包括国家 ADR 数据库和国家 ADR 咨询委员会)。

然而，这种以 SRS 为中心的方法近年来受到了很多批评。自发报告可能不完整或不正确，有偏见(导致少报 ADR)，并且实现缓慢。此外，SRS 本质上是一种[被动](https://globalpharmacovigilance.tghn.org/articles/spontaneous-reporting/#:~:text=Spontaneous%20reporting%20has%20been%20defined%20as%3A&text=Spontaneous%20reporting%20is%20by%20nature,local%20or%20national%20pharmacovigilance%20centre.)信号检测方法，依靠消费者向卫生当局报告可疑的 ADR(导致只有很小比例的不良事件被报告)。

SRS 也没有考虑围绕 ADR 的大量且不断增长的其他有价值的信息，如医学文献和来自社交媒体和博客的数据。

其他新兴因素使行业的传统光伏方式变得复杂，包括:

1.  **在新兴市场实施 SRS**:实施有效的 SRS 战略可能会很困难，尤其是在中低收入地区，那里有许多偏远地区、落后的电信基础设施和拼凑的医疗保健系统。
2.  **立法**:越来越严格的审查导致了在[欧洲](https://www.ema.europa.eu/en/pharmacovigilance-legislation)、[加拿大](https://www.canada.ca/en/health-canada/services/drugs-health-products/compliance-enforcement/good-manufacturing-practices/guidance-documents/pharmacovigilance-guidelines-0102.html)和[美国](https://www.lexology.com/library/detail.aspx?g=be199577-497a-4986-a602-a4b4a1085b1b)等司法管辖区围绕光伏实践的相对较新的立法。这项立法越来越多地要求制药公司筛选科学文献和其他来源(以及 SRS)。
3.  **大量(且不断增加的)非结构化数据**:分析医学文献很容易，但另一件事要做——尤其是使用手动方法。医学文献呈指数增长，[每隔几个月左右数量就翻一番。这是一个巨大的潜在的 ADR 信号，但也是一个巨大的挑战，药物警戒官员的任务是涉水通过所有这些信息。](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3116346/)

[在欧盟](https://www.ema.europa.eu/en/documents/regulatory-procedural-guideline/guideline-good-pharmacovigilance-practices-gvp-module-vi-collection-management-submission-reports_en.pdf)，制药公司必须至少每周一次重新访问这一不断增长的海量数据，“通过对广泛使用的参考数据库(如 Medline、Pubmed 或 Embase)进行系统的文献综述，保持对可能出版物的了解。”

除了主要数据库中的主流科学文献之外，制造商还被期望[监控销售其产品的地区的本地出版物。](https://pharma.elsevier.com/pharmacovigilance/screening-scientific-literature-for-adverse-drug-reactions/)

## 通过 ML 和 NLP 实现自动化可以提高药物警戒和医学文献综述

快速增长的医疗数据量与不断扩大的监管要求相结合，给制药公司带来了更大的压力，他们需要找到更有效的方法来进行 PV 审查。“对于研究人员、科学家和医生来说，阅读和处理大量的科学文章并掌握最重要的信息是不可能的，”Tafti 等人解释道。“因此，迫切需要开发智能计算方法，特别是大数据分析解决方案，以高效处理这些大量数据。”

分析大量医疗数据(包括科学文献和 SRS，以及临床试验、流行病学数据库、社交媒体信号和其他来源)的一个有效解决方案是部署机器学习(ML)和自然语言处理(NLP)模型。

现实世界的科学研究已经证实了这种方法的有效性。Tafti 等人。(2017)开发了一个可扩展的文本挖掘解决方案(由 Apache Spark、ML 和 NLP 模型以及 Elasticsearch NoSQL 分布式数据库支持)，用于分析生物医学文章和健康相关的社交媒体帖子。

该项目有三层自动化:

1.  **数据收集**:部署了一个网络爬虫来收集 XML 文件形式的数据，然后将这些数据转换成带有相关元数据(出版物、作者等)的纯文本文件。)并存储在 NoSQL 数据库中。
2.  **NLP** :两种不同应用中的自然语言处理:(a)选择相关文档(随着系统积累数据，它建立了确保高质量结果的标准)。(b)文本处理:在文本标准化之后，文件被转换成一组单独的句子，以确保模型的准确性。然后，人类对随机的句子组进行注释，以进一步完善模型。
3.  **ML** :预测模型然后基于两种状态之一对文档进行分类:包括 ADR 或不包括 ADR。

在分析了来自 PubMed Central、MedHelp 和 WebMD 等来源的数百篇文章和帖子后，该解决方案实现了 92.7%的准确率，93.6%的精确度和 93%的召回率。

“这项工作不仅从大数据生物医学文献中检测和分类了 ADE 句子，还科学地可视化了 ADE 交互，”根据作者的说法。

## 与 CapeStart 的 ML 和 NLP 团队一起改进您的药物警戒医学文献综述

CapeStart 的机器学习和自然语言处理专家已经与数十家生命科学和制药公司合作，以改善和简化他们的光伏流程。从数据收集和注释到高度复杂的 ML 和 NLP 模型的开发和部署，这些模型旨在挖掘堆积如山的医学文献， [CapeStart 可以提高](https://www.capestart.com/solutions/nlp-aided-systematic-literature-review/)药物警戒文献综述的速度、成本和准确性。

请立即联系我们，安排一次与我们专家的简短咨询。