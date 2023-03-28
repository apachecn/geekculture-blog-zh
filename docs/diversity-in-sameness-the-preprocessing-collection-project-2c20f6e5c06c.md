# 预处理集合项目

> 原文：<https://medium.com/geekculture/diversity-in-sameness-the-preprocessing-collection-project-2c20f6e5c06c?source=collection_archive---------7----------------------->

## 用于各种特征工程的各种转换器的库

![](img/da48ef69bea3e3f573374700e70650d0.png)

Picture by the author — actually by stable diffusion

通常情况下，您会期望一些关于某些类型的预处理或类似的东西在这里提到。但今天应该是关于别的事情。

# 再利用和回收♺

数据预处理在每个数据科学家的日常生活中扮演着重要的角色。在这里格式化一列，在那里转换值，等等。你知道我的意思。很好，有熊猫，雪鸡，等等。如果您使用表格数据，您会很高兴不时地再次使用相同的预处理方法。scikit-learn 的管道有助于使预处理步骤形式化并易于实现。毕竟，项目和过程的结构在某种程度上是相似的:一个 *X* 和一个*y*,*X*被转换——有时安装一个编码器或类似的东西，最后，预处理步骤的*最终产品*在一个模型中结束。当然，这过于简化了复杂性。但我想你明白我的意思了。如果做得好，可以使用导致生产系统中一对一训练的预处理步骤。

作为一名软件开发人员，作为一个不想彻底毁掉这个星球的人，我学到了:

> “再利用和回收”

这就是为什么当我可以回收我的预处理代码时，我特别高兴。在内部，我们有一个小的包，在这里我们收集了——或多或少地组织起来——所有的预处理方法。当然，重要的是实现总是有一些清晰和一致的 API。否则，回收就成了一件痛苦的事，也不会带来所希望的简单易用。

不久前，我开始寻找类似的步骤。当然，你也可以在浩瀚的互联网中找到它们:[Feature-engine](https://feature-engine.readthedocs.io/en/latest/)——一个伟大的项目，在此仅作为众多项目之一提及。但是，我没有找到一个或多或少简单的预处理方法的集合，其中也包含更具体的步骤。

**为什么没有一个简单的预处理方法集合，让每个人都可以方便快捷地做出贡献？**

> 一个形式化的集合，因此它易于使用，易于实现，并且可以将各个步骤组合起来以适应更复杂的预处理需求。这是一个集合，参与快速且不复杂，其他数据科学家不仅可以找到其他预处理步骤，还可以从其他人的想法中获得灵感:“啊，这是一个很好的主意，我也将在我的数据上尝试一下”。

因为我没有发现这样的东西。只好自己动手: [**专题-改版**](https://github.com/chrislemke/feature-reviser) 。

就目前而言，收集的内容相当少，但我认为我们可以一起收集许多有用的想法，并相互启发。因为即使数据集和项目是多样化的——不知何故，许多代码可以在项目、公司和人之间共享。因此，请随意重复使用、回收和[贡献](https://chrislemke.github.io/feature-reviser/CONTRIBUTING/)！

## 简短的见解

scikit-learn 的*fit*/*transform*API 已经被广泛使用。无论是 [XGBoost](https://xgboost.readthedocs.io/en/stable/) 、 [CatBoost](https://catboost.ai/) ，还是 Feature-engine 的方法——都遵循这个结构。集合的预处理步骤也是如此。这里有一个过于简单的例子:

```
import pandas as pd
from sklearn.base import BaseEstimator, TransformerMixinclass DummyTransformer(BaseEstimator, TransformerMixin): def __init__(self, string_to_replace: str, column: str) -> None:
        self.string_to_replace = string_to_replace
        self.column = column def fit(self, X=None, y=None) -> "DummyTransformer":
        return selfdef transform(self, X: pd.DataFrame) -> pd.DataFrame:
    X = X.copy() X[self.column] = X[self.column].replace(self.string_to_replace,    "DUMMY!") return X
```

是啊。对于那些不知道的人来说，形式化预处理步骤并使其与 scikit-learn 等兼容是如此容易。

但是读够了。看看[项目](https://github.com/chrislemke/feature-reviser)，看看[变压器集合](https://github.com/chrislemke/feature-reviser/tree/main/feature_reviser/transformer)。使用它，[表达并分享你的预处理想法](https://chrislemke.github.io/feature-reviser/CONTRIBUTING/)，查看[文档](https://chrislemke.github.io/feature-reviser/)并开始回收代码！我们可以一起让每个人的数据科学生活变得更好。

感谢阅读！