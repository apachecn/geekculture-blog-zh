# 评估多类分类和实现的度量

> 原文：<https://medium.com/geekculture/evaluating-metrics-for-multi-class-classification-and-implementations-be389cb17e65?source=collection_archive---------5----------------------->

![](img/c994c0a676a07b34d6689741a018196d.png)

Image from Google

这篇博客是这篇文章的延续。

到目前为止，您已经了解了二进制分类指标。从现在开始，我们将学习多类分类的度量，在下一篇文章中，您将学习多标签分类及其度量。

多类分类无非是解决一个分类问题，其中我们在目标列中有多个类(类的数量> 2)。例如，将水果图像分类为苹果、桔子和香蕉。

我在这里附上一个示例数据集。在该数据集中，发展指数列是具有多个类的目标(类的数量> 2)。

正如我们在二进制分类度量学过的那样，有了它们，我们就可以制定多分类度量。

在多分类指标中，每个二元分类指标有 3 种实现方式。我写的时候你可能会明白…

对于精确二进制分类度量，在多分类度量中，我们有三个版本，

1.  微平均精度
2.  宏观平均精度
3.  加权精度

作为精度，我们在多类分类中有 3 个版本的召回二进制分类度量。

1.  微观平均回忆
2.  宏观平均回忆
3.  加权回忆

如上所述，我们可以将每个二进制分类度量分为 3 个版本，并将其命名为多分类度量。

现在，我将讨论精度，并添加召回代码，所有剩余的指标将以相同的方式构建(下面的精度遵循相同的过程)。在未来的帖子中，我将向您展示如何在多类分类中形成 ROC-AUC。

注意:-不要跳进任何东西，除非你知道基本知识。因为它可能会吞噬你的信心和信念。

让我们讨论一下 precision 的 3 个版本。

1.  **微平均精度:-** 在这里，首先，我们将找到所有类的所有真阳性，并将它们相加，以获得所有类的总真阳性。之后，我们将找到所有类的所有误报，并将它们相加，得到所有类的总误报。现在，计算真阳性和假阳性总数的精度。

2.**宏平均精度** :-这里，我们将找到目标列中不同类的精度，然后对它们进行平均以获得最终精度。

3.**加权精度** :-这里，加权精度与宏平均精度相同，但区别就在这里，它取决于每类中存在的样本数并根据样本数进行加权。

我知道，我已经为每一个给出了小的定义，但这就是定义，代码解释了一切。所以，检查定义，看看定义所需的代码，了解代码中发生了什么，你就会明白事情。

请记住，当数据平衡时，我们可以使用宏观版本，但当数据不平衡时，微观和加权版本更可取。

请注意，下面的代码由 precision 的所有 3 个版本组成。

```
import pandas
import argparse
import numpy
from collections import Counterfrom sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn import metrics#there are 3 Types of precision in case of Multi-class classification. 
#1\. Macro averaged precision
#2\. Micro averaged precision
#3\. Weighted precisiondef true_positive(y_true, y_pred):
    tp = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 1 and yp == 1:
            tp += 1
    return tp

def true_negative(y_true, y_pred):
    tn = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 0 and yp == 0:
            tn += 1
    return tn

def false_positive(y_true, y_pred):
    fp = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 0 and yp == 1:
            fp += 1
    return fp

def false_negative(y_true, y_pred):
    fn = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 1 and yp == 0:
            fn += 1
    return fndef precision(y_test, y_pred):
    tp =true_positive(y_test, y_pred)
    fp = false_positive(y_test, y_pred)
    try:
        return(tp/(tp+fp))
    except ZeroDivisionError:
        return 0def Macro_averaged_precision(y_test, predictions):
    precisions = []
    for i in range(1,5):
        temp_ytest = [1 if x == i else 0 for x in y_test]
        temp_ypred = [1 if x == i else 0 for x in predictions]
        print(temp_ypred)
        print(temp_ytest)
        prec = precision(temp_ytest, temp_ypred)
        precisions.append(prec)

    return (sum(precisions)/len(precisions))

def Micro_averaged_precision(y_test, predictions):
    tp = 0
    fp = 0
    for i in range(1,5):
        temp_ytest = [1 if x == i else 0 for x in y_test]
        temp_ypred = [1 if x == i else 0 for x in predictions] tp += true_positive(temp_ytest, temp_ypred)
        fp += false_positive(temp_ytest, temp_ypred) precisions = tp / (tp + fp) return precisionsdef weighted_precision(y_test, predictions):
    num_classes = len(numpy.unique(y_test))
    #coutns for every class
    precision = 0
    for i in range(1, num_classes):
        temp_ytest = [1 if x == i else 0 for x in y_test]
        temp_ypred = [1 if x == i else 0 for x in predictions] tp = true_positive(temp_ytest, temp_ypred)
        fp = false_positive(temp_ytest, temp_ypred)

        try:
            preai = tp / (tp+fp)
        except ZeroDivisionError:
            preai = 0 weighted = preai*sum(temp_ytest) precision += weighted precision = precision/len(y_test)
    return precisionif __name__ == "__main__":

    data = pandas.read_csv("C:\\Users\\iamvi\\OneDrive\\Desktop\\Metrics_in_Machine_Learning\\development-index\\Development Index.csv")

    train = data.drop(['Development Index'], axis = 1).values
    test = data["Development Index"].valuesmodel = LogisticRegression()X_train, X_test, y_train, y_test = train_test_split(train, test, stratify = test)model.fit(X_train, y_train)
    predictions = model.predict(X_test)print("Macro precision is:", Macro_averaged_precision(y_test, predictions))
    print("Micro precision is:", Micro_averaged_precision(y_test, predictions))
    print("Weighted precision is:", weighted_precision(y_test, predictions))
    print("sklearn Macro", metrics.precision_score(y_test, predictions, average = "macro"))
    print("sklearn Micro", metrics.precision_score(y_test, predictions, average = "micro"))
    print("sklearn weighted", metrics.precision_score(y_test, predictions, average = "weighted"))
```

你可能会发现由于介质接口导致的不一致的缩进，很难看到代码，点击[此处](https://github.com/VishnuVardhanVarapalli/Classification-Metrics/blob/main/Multi_class_classification_precision.py)查看 Github 中的代码。

这就是所有多类分类度量是如何从二进制分类度量产生的。

正如我所说的，我会给你的代码召回类似精度，以及所有其余的指标。

```
import pandas
import argparse
import numpy
from collections import Counterfrom sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn import metrics#there are 3 Types of recall in case of Multi-class classification. 
#1\. Macro averaged recall
#2\. Micro averaged recall
#3\. Weighted recalldef true_positive(y_true, y_pred):
    tp = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 1 and yp == 1:
            tp += 1
    return tp

def true_negative(y_true, y_pred):
    tn = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 0 and yp == 0:
            tn += 1
    return tn

def false_positive(y_true, y_pred):
    fp = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 0 and yp == 1:
            fp += 1
    return fp

def false_negative(y_true, y_pred):
    fn = 0
    for yt, yp in zip(y_true, y_pred):
        if yt == 1 and yp == 0:
            fn += 1
    return fndef recall(y_test, y_pred):
    tp = true_positive(y_test, y_pred)
    fn = false_negative(y_test, y_pred)
    return(tp/(tp+fn))def Macro_averaged_recall(y_test, predictions):
    recalls = []
    for i in range(1,5):
        temp_ytest = [1 if x == i else 0 for x in y_test]
        temp_ypred = [1 if x == i else 0 for x in predictions]
        print(temp_ypred)
        print(temp_ytest)
        rec = recall(temp_ytest, temp_ypred)
        recalls.append(rec)

    return (sum(recalls)/len(recalls))

def Micro_averaged_recall(y_test, predictions):
    tp = 0
    tn = 0
    for i in range(1,5):
        temp_ytest = [1 if x == i else 0 for x in y_test]
        temp_ypred = [1 if x == i else 0 for x in predictions] tp += true_positive(temp_ytest, temp_ypred)
        tn += true_negative(temp_ytest, temp_ypred) recall = tp / (tp + tn) return recalldef weighted_recall(y_test, predictions):
    num_classes = len(numpy.unique(y_test))
    #counts for every class
    recall = 0
    for i in range(1, num_classes):
        temp_ytest = [1 if x == i else 0 for x in y_test]
        temp_ypred = [1 if x == i else 0 for x in predictions]tp = true_positive(temp_ytest, temp_ypred)
        tn = true_negative(temp_ytest, temp_ypred)

        try:
            rec = tp / (tp+tn)
        except ZeroDivisionError:
            rec = 0weighted = rec*sum(temp_ytest)recall += weightedrecall = recall/len(y_test)
    return recallif __name__ == "__main__":

    data = pandas.read_csv("C:\\Users\\iamvi\\OneDrive\\Desktop\\Metrics_in_Machine_Learning\\development-index\\Development Index.csv")

    train = data.drop(['Development Index'], axis = 1).values
    test = data["Development Index"].valuesmodel = LogisticRegression()X_train, X_test, y_train, y_test = train_test_split(train, test, stratify = test)model.fit(X_train, y_train)
    predictions = model.predict(X_test)print("Macro recall is:", Macro_averaged_recall(y_test, predictions))
    print("Micro recall is:", Micro_averaged_recall(y_test, predictions))
    print("Weighted recall is:", weighted_recall(y_test, predictions))
    print("sklearn Macro", metrics.recall_score(y_test, predictions, average = "macro"))
    print("sklearn Micro", metrics.recall_score(y_test, predictions, average = "micro"))
    print("sklearn weighted", metrics.recall_score(y_test, predictions, average = "weighted"))
```

precision 的 3 个版本的定义也将应用于召回。

当你在 3 个版本中实现了我上面所说的所有多类分类度量标准之后，你可以自信地说“我知道多类分类”。😉

检查[分类-度量库](https://github.com/VishnuVardhanVarapalli/Classification-Metrics)以获得我解释的所有概念的代码。当我在这里写文章时，GitHub 将会更新代码。所以，把叉子放在上面。

如果你想在这里补充什么，或者想让我解释这里遗漏的概念，让我从[这里](https://iamvishnuvarapally.wixsite.com/port-folio)知道。

可以在不同平台 [LinkedIn](https://www.linkedin.com/in/vishnu-vardhan-varapalli-b6b454150/) 、 [Github](https://github.com/VishnuVardhanVarapalli) 、 [medium](https://iamvishnu-varapally.medium.com/) 关注我。

快乐的 Learning✌.