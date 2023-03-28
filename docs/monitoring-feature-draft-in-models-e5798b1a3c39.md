# 监控模型中的特征草图

> 原文：<https://medium.com/geekculture/monitoring-feature-draft-in-models-e5798b1a3c39?source=collection_archive---------10----------------------->

## 模型部署后跟踪数据变化(漂移)的方法

![](img/bc4bccff0db5e2781c139104dd322835.png)

From [Pexels Photos](https://www.pexels.com/ko-kr/photo/9456595/). Photo classified under the “royalty free” section. Also labeled as “free for use”.

# 什么是模型漂移？

您已经完成了一个奇妙的模型的制作，它甚至在各种验证集上都表现得非常好。您已经准备好为真实数据部署模型了。然而，在部署之后，您发现模型的性能…