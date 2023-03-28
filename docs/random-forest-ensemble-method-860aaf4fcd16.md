# 随机森林集合方法

> 原文：<https://medium.com/geekculture/random-forest-ensemble-method-860aaf4fcd16?source=collection_archive---------7----------------------->

监督学习算法中最常用于回归和分类问题的任何数据(也用于非线性数据或实时数据)的高级技术之一是随机森林。顾名思义，这是一个由许多树木组合成的随机森林。

好吧，什么是合奏法？

*集成方法是一种元算法，它将几种机器学习技术结合到一个预测模型中，以减少方差(bagging)、偏差(boosting)或改善预测(stacking)。*

*集成方法是构建一组预测模型并将它们的输出组合成单个预测的机器学习方法。将几个模型组合在一起的目的是为了获得更好的预测性能，在许多情况下，集成模型比单个模型更准确。虽然在 20 世纪 70 年代已经完成了一些关于集合方法的工作，但是直到 20 世纪 90 年代，以及诸如 bagging 和 boosting 等方法的引入，集合方法才开始被更广泛地使用。今天，它们代表了一种标准的机器学习方法，无论何时需要良好的预测准确性，都必须考虑这种方法。*

是的，正如所陈述的，当 ML 和高级学习方法的应用增加时，集成方法被广泛使用。由于集合方法会多次重复该过程，因此与模型使用 bagging 或 boosting 时相比，模型会学习数据并做出正确的预测。当集合方法使用 bagging 或 boosting 时，它会比模型不使用它时对做出正确的预测产生更大的影响。例如，当使用线性回归或逻辑回归或决策树时，它只在整个集合上训练一次，而在集成方法中，它会多次执行任务，就像人们多次做任何活动时，他们就会成为这方面的专家。

> 实践使人完美。难道不是吗，当事情以正确的方式进行而不放弃的时候，有一天任务会变得容易得多，对你来说也不再困难。

随机森林就是组合在一起给出一个输出或结果的一组决策树。ML 中的决策树是做什么的？如果这是你的问题，你必须先去决策树，因为没有主要成分或基础就没有树。[点击此处了解决策树](http://Ensemble methods are machine learning methods that construct a set of predictive models and combine their outputs into a single prediction. The purpose of combining several models together is to achieve better predictive performance, and it has been shown in a number of cases that ensembles can be more accurate than single models. While some work on ensemble methods has already been done in the 1970s, it was not until the 1990s, and the introduction of methods such as bagging and boosting, that ensemble methods started to be more widely used. Today, they represent a standard machine learning method which has to be considered whenever good predictive accuracy is demanded.)

一棵决策树是如何构建的？是的，你现在知道了。它使用基尼系数、熵以及用于分裂根节点的任何具有更高信息增益的因素进行分裂，该过程继续进行以形成决策树。这样的决策树被组合形成随机森林，该随机森林使用引导+聚集的打包技术。

简而言之，Booststraping 就是用替换从数据中抽取一个样本，并将所有这些从总体中抽取的样本组合起来，称为 Bagging。Bagging 是一种集合技术，通过收集不同的样本来做出决策。在随机森林中，我们从总体中选择这样的样本，并形成决策树，而不是一个决策树。这里有许多由不同样本(bootstrap 样本)形成的树，这些样本组合在一起形成随机森林。由“n”个决策树做出的所有预测的平均值为随机森林做出预测。这种类型的集成被称为并行学习，而 boosting 使用顺序学习。平行表示当数据在不同样本上训练时，基础学习者(每个决策树)之间存在独立性，然后获得所有分数以找到最优分数。Sequential 表示所有 boosting 算法使用的基础学习器之间存在相关性。随机森林仅使用 bagging 技术来减少数据集中的差异。

![](img/0be50e62938b8c5cf090e3a616ab4405.png)

Random Forest diagram

Bagging 有助于减少数据中的差异，因为有许多决策树，学习增加，所以数据被很好地训练，也有可能过度拟合。在 **bagging** 中，在训练数据的不同样本上训练的多个分类器的输出被组合，这有助于减少总体方差。

当随机森林已知时，实现只需要几行代码。

```
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state = 0)
model = RandomForestRegressor(n_estimators = 10, random_state = 0)
model.fit(X_train, y_train)
model.predict(X_test)
```

这里 n_estimators 是要为这个模型构建的树的数量，random_state 是从总体中选取一个样本，如果使用相同的随机状态，则在任何时候执行时都会选取样本。😮这么有用的参数，是的！！

要了解更多关于随机森林中的参数，请参考这个 sklearn 网站的[分类器](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)和[回归器](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)

随机森林的优点和缺点:

优点:-

1.  它减少了决策树中的过拟合，有助于提高准确性
2.  它对分类和回归问题都很灵活
3.  它适用于分类值和连续值
4.  它自动处理数据中缺失的值
5.  不需要数据标准化，因为它使用基于规则的方法。
6.  它有能力处理更高维度的大型数据集。它可以处理成千上万的输入变量并识别最重要的变量，因此被认为是降维方法之一。此外，该模型输出变量的重要性，这对于特征选择是有用的
7.  它有方法来平衡数据集中类不平衡的错误。
8.  这里在 bootstrap 抽样中，三分之一(比如说)的数据不用于训练，可以用于测试。这些被称为袋外样品。对这些袋外样本估计的误差称为袋外误差。出袋估计与使用与训练集大小相同的测试集一样准确。因此，使用袋外误差估计消除了对预留测试集的需要。

反对意见:-

1.  它确实在分类方面做得很好，但是在回归问题上做得不好，因为它不能给出精确连续的自然预测。在回归的情况下，它不会预测超出训练数据的范围，并且它们可能会过度拟合特别嘈杂的数据集。
2.  对于统计建模者来说，随机森林感觉像是一种黑盒方法，因为对模型做什么几乎没有控制。你最多可以尝试不同的参数和随机种子。由于决策树的集合，它也遭受可解释性和不能确定每个变量的重要性。
3.  它需要大量的计算能力和资源，因为它构建了大量的树来组合它们的输出。
4.  它还需要大量的训练时间，因为它结合了大量的决策树来确定类别。

![](img/fa9f0b5a57869e58bc42c2e4f4e108c8.png)

Photo by [Baciu Cristian Mihai](https://unsplash.com/@vansolo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 坚强地站着，相信自己，追逐梦想

> 除了你对自己想法的限制之外，你的成就没有限制。无限。有勇气去做不可能的事，并产生影响。

如果你学到了什么，请告诉我，给我一些支持，并与有用的人分享。如果你有任何疑问或想说什么，请在评论框中告诉我。给世界带来一些光明。祝你有愉快的一天。🥰