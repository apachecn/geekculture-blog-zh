# 机器学习中的 XGBoost

> 原文：<https://medium.com/geekculture/xgboost-in-machine-learning-586f2c56e8d8?source=collection_archive---------9----------------------->

![](img/c68c7bc3368d09d2ee5fb0c358189873.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

本文将讨论 XGBoost 算法，并构建和优化模型以获得最佳结果。

这是上一篇 [**的延续部分**](/@abhi2652254/cross-validation-in-machine-learning-483ae08faf3f?source=your_stories_page-------------------------------------) 引自原**[**科目**](https://www.kaggle.com/learn/intermediate-machine-learning) **。****

****免责声明:** XGBoost 算法在 Kaggle 比赛中以将竞争对手带到**排行榜**而闻名。**

****梯度推进**是一种迭代循环的方法，从而将模型添加到集合中。**

**它从用一个模型初始化集合开始，这个模型的预测可能非常简单明了**。(即使预测非常不准确，对系综的后续添加**将解决这些错误。)******

**然后，循环开始如下:**

*   **首先，**当前集合**用于为数据集中的每个观察值生成预测。为了进行预测，将来自**集合**中所有模型的**预测相加**。**
*   **这些预测然后用于计算一个**损失函数**(假设 [**均方误差**](https://en.wikipedia.org/wiki/Mean_squared_error) )。**
*   **然后，损失函数用于拟合将被添加到集合的新模型**。具体来说，确定**模型参数**，使得**将这个新模型添加到集合**将减少损失。(注:**梯度提升**中的**梯度**是指在**损失函数**上使用[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)来确定这个新模型中的**参数**。)****
*   **最后，将**新型号**添加到整体中，并重复该过程。**

**我会写代码来一步步走完对 [**XGBoost**](https://stackabuse.com/bytes/end-to-end-xgboost-regression-pipeline-with-scikit-learn/) **的理解过程。****

```
import pandas as pd
from sklearn.model_selection import train_test_split# Read the data
X = pd.read_csv('../input/train.csv', index_col='Id')
X_test_full = pd.read_csv('../input/test.csv', index_col='Id')# Remove rows with missing target, separate target from predictors
X.dropna(axis=0, subset=['SalePrice'], inplace=True)
y = X.SalePrice    

X.drop(['SalePrice'], axis=1, inplace=True)# Break off validation set from training data
X_train_full, X_valid_full, y_train, y_valid = train_test_split(X, y, train_size=0.8, test_size=0.2,random_state=0) # "Cardinality" means the number of unique values in a column
# Select categorical columns with relatively low cardinality (convenient but arbitrary)low_cardinality_cols = [cname for cname in X_train_full.columns if X_train_full[cname].nunique() < 10 and X_train_full[cname].dtype == "object"]# Select numeric columns
numeric_cols = [cname for cname in X_train_full.columns if X_train_full[cname].dtype in ['int64', 'float64']]# Keep selected columns only
my_cols = low_cardinality_cols + numeric_cols
X_train = X_train_full[my_cols].copy()
X_valid = X_valid_full[my_cols].copy()
X_test = X_test_full[my_cols].copy()# One-hot encode the data (to shorten the code, we use pandas)
X_train = pd.get_dummies(X_train)
X_valid = pd.get_dummies(X_valid)
X_test = pd.get_dummies(X_test)
X_train, X_valid = X_train.align(X_valid, join='left', axis=1)
X_train, X_test = X_train.align(X_test, join='left', axis=1)
```

**上面使用的代码是从我以前使用的以前的模块中继承来的(请检查我以前的 [**部分** )](/@abhi2652254) 。**基数**用于具有类别< 10 **的**分类列**。80%培训**和 **20%测试数据**准备完毕。**数字列**也被使用。最后， **X_train、X_test 和 X_valid** 被创建。它们是**训练集、测试集和验证集。****

**现在让我们构建下面的 XGBoost 模型。**

```
from xgboost import XGBRegressor# Define the model
my_model_1 = XGBRegressor(random_state = 0)# Fit the model
my_model_1.fit(X_train, y_train)
```

**在这种情况下，**随机种子** ( **random_state** )被设置为 0。**

**模型预测和平均绝对误差( [**MAE**](https://en.wikipedia.org/wiki/Mean_absolute_error) )为**

```
**from** sklearn.metrics **import** mean_absolute_error​*# Get predictions*predictions_1 **=** my_model_1.predict(X_valid)# Calculate MAE
mae_1 = mean_absolute_error(predictions_1, y_valid) # Your code here# Uncomment to print MAE
print("Mean Absolute Error:" , mae_1) Mean Absolute Error: 17662.73
```

**我们有了带有**默认参数**的**基线**模型结果。现在，让我们**改进模型**，看看 MAE 值是否可以降低。**

**修改后的模型的代码是**

```
# Define the model
my_model_2 = XGBRegressor(n_estimators=600,learning_rate=0.05) # Fit the model
my_model_2.fit(X_train, y_train) # Your code here# Get predictions
predictions_2 = my_model_2.predict(X_valid) # Your code here# Calculate MAE
mae_2 = mean_absolute_error(predictions_2, y_valid) # Your code here# Uncomment to print MAE
print("Mean Absolute Error:" , mae_2)Mean Absolute Error: 16716.16
```

**上面使用的参数是**

## **`n_estimators`**

**`n_estimators`指定建模循环的次数。它等于集合中包含的**个型号**的数量。**

*   **太低的*a 值导致*欠拟合*，从而导致**对训练数据和测试数据的预测**不准确。***
*   ***a 值过高*导致*过拟合*，导致**对训练数据**预测准确，但对测试数据预测不准确。**

**典型值范围从 100-1000，尽管这很大程度上取决于下面讨论的`learning_rate`参数。**

## **`learning_rate`**

**不是通过**将来自每个组件模型的预测**相加来获得预测，而是在将它们相加之前，可以将来自每个模型的预测**乘以**小数值**(称为**学习率**)。****

**这意味着每增加一棵树，帮助就少一点。因此，可以在不过度拟合的情况下设置更高的值`n_estimators`。**

**一般来说，**小的学习率**和**大数量的估计器**将产生更**精确的 XGBoost 模型**，尽管它也将花费模型**更长的时间来训练**，因为它在周期中进行更多的迭代。默认情况下，XGBoost 设置`learning_rate=0.1`。上述 MAE 值为 16716.16，比之前的 MAE 值(17662.73)有所提高。当然，可以迭代**各种参数值**以达到**更低的 MAE 数**。**

****打破模式****

**本教程的最后一步是打破模型，意思是使用**某些参数值来增加 MAE** (大于 17662.73)。虽然我们总是以降低 MAE 值为目标，但这种方式将提供一种利用各种参数并通过迭代得出合理的 MAE 值的思路。**

```
# Define the model
my_model_3 = XGBRegressor(n_estimators=200,learning_rate=0.01)# Fit the model
my_model_3.fit(X_train, y_train) # Your code here# Get predictions
predictions_3 = my_model_3.predict(X_valid)# Calculate MAE
mae_3 = mean_absolute_error(predictions_3, y_valid)# Uncomment to print MAE
print("Mean Absolute Error:" , mae_3) Mean Absolute Error: 29111.51
```

**MAE 值在 **29111** 左右，即> 17662.73。**

****提前停止轮次**的参数也被使用，定义如下**

## **`early_stopping_rounds`**

**`early_stopping_rounds`用于自动找到`n_estimators`的理想值。早期停止会导致模型在验证分数停止提高时停止迭代，即使`n_estimators`没有任何硬停止。最好为`n_estimators`设置一个较高的值，然后使用`early_stopping_rounds`来寻找停止迭代的最佳时间。**

**在这里，我将结束 XGBoost 教程，尽管从长远来看，它需要大量的实践。我将写下一篇关于**数据泄露**的文章，并结束课程，让即将到来的**端到端项目**的**管道**上线。**

**敬请期待，在此之前，请查看我的其他 [**文章**](/@abhi2652254) ，说 [**你好**](https://www.linkedin.com/in/obhinaba17/) ，，并让我知道你是否想要一位技术作家为你写作，我们可以通过一个简短的电话进行讨论。**