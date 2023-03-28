# Sklearn 期望数据处于良好状态

> 原文：<https://medium.com/geekculture/sklearn-expects-data-to-be-in-shape-64fbcaf80a8c?source=collection_archive---------5----------------------->

## Sklearn 需要一个 2D 数组

![](img/80b44c5393dfd8480de34c13ed82ccf0.png)

Photo by [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**问题陈述:**下面 sklearn 错误说明了一切。

```
ValueError: Expected 2D array, got 1D array instead: array=[2 7 8 4 1 6]. Reshape your data either using array.reshape(-1, 1) if your data has a single feature or array.reshape(1, -1) if it contains a single sample.
```

让我们认识到在构建一个基本的 sklearn 线性回归模型时，使数据成形的重要性。

导入必要的库，

```
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
```

创建一个样本数据框，

```
data = {'X':[1,2,3,4,5,6,7,8,9,10],
        'y':[5,7,9,11,13,15,17,19,21,23]}
df = pd.DataFrame(data)
```

从数据帧 df 中获取 X，y 数据，

```
X = df["X"]
y = df["y"]
```

执行[列车试运行](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)，

```
X_Train, X_Test, Y_Train, Y_Test = train_test_split(X, y, test_size = 1/3, random_state = 0)
```

建立一个基本的[线性回归模型](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)，

```
# instantiate the model with desired parameter values
lr = LinearRegression()# fit the model to the training data
lr.fit(X_Train, Y_Train)# apply the model to test data
y_pred = lr.predict(X_Test) # get coef, intercept values
print (lr.coef_)
print (lr.intercept_)
```

这里有一个错误！

```
ValueError: Expected 2D array, got 1D array instead: array=[2 7 8 4 1 6]. Reshape your data either using array.reshape(-1, 1) if your data has a single feature or array.reshape(1, -1) if it contains a single sample.
```

在遇到这个错误之前，我从来不知道我们需要 2D 数组来获得 sklearn 工作。让我们调试错误。该错误指出它需要 2D 数组，但却得到了 1D 数组。X 的形状给出了(10)的输出，这显然是一个 1D 数组。

```
X.shapeOutput: (10,)
```

我们用 X = df["X"]得到 X 变量。这将生成一个形状为(10，)没有任何列的序列。有很多方法可以将 2D 数组赋值给 x。让我们来看看其中的几种

```
#Option 1 - Reshaping to (-1,1)
X = np.array(X).reshape(-1,1)#Option 2 - Getting as a dataframe
X = df.iloc[:, :-1].values#Option 3 - Getting as a dataframe
X = df[["X"]]
```

一旦我们修改了 X，剩下的代码运行良好，并给出了输出。

**思考内容:**无论何时使用 sklearn，在模型中使用它之前，请确保将 2D 数组分配给预测变量。