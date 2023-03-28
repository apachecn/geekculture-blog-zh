# LightGBM 启动代码

> 原文：<https://medium.com/geekculture/lightgbm-starter-code-ea6377c97039?source=collection_archive---------23----------------------->

这是你的第一个 LightGBM 代码！

![](img/c87fc4b327ae19d61dee4ff2b712a888.png)

Photo by [Bench Accounting](https://unsplash.com/@benchaccounting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/online-classes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

LightGBM 是一个梯度推进框架，使用基于树的学习算法。它被设计为分布式和高效的，具有以下优点:

*   训练速度更快，效率更高
*   更低的内存使用率
*   更高的精确度
*   支持并行和 GPU 学习
*   能够处理大规模数据

```
import os *#to access files*
import pandas as pd *#to work with dataframes*
import numpy as np *#just a tradition*
from sklearn.model_selection import StratifiedKFold *#for cross-validation*
from sklearn.metrics import roc_auc_score *#this is we are trying to increase*
import matplotlib.pyplot as plt *#we will plot something at the end)*
import seaborn as sns *#same reason*
import lightgbm as lgb *#the model we gonna use*
```

让我们来读数据:训练，目标和测试

```
train = pd.read_csv('../input/train.csv')
test = pd.read_csv('../input/test.csv')

for c in train.columns:
    if train[c].dtype == 'object':
        lbl = LabelEncoder() 
        lbl.fit(list(train[c].values) + list(test[c].values)) 
        train[c] = lbl.transform(list(train[c].values))
        test[c] =  lbl.transform(list(test[c].values))
```

数据集的形状

```
print('Shape train: {}\nShape test: {}'.format(train.shape, test.shape))n_comp = 6

# PCA
pca = PCA(n_components=n_comp, random_state=42)
pca2_results_train = pca.fit_transform(train.drop(["y"], axis=1))
pca2_results_test = pca.transform(test)

# ICA
ica = FastICA(n_components=n_comp, random_state=42)
ica2_results_train = ica.fit_transform(train.drop(["y"], axis=1))
ica2_results_test = ica.transform(test)

# Append decomposition components to datasets
for i in range(1, n_comp+1):
    train['pca_' + str(i)] = pca2_results_train[:,i-1]
    test['pca_' + str(i)] = pca2_results_test[:, i-1]

    train['ica_' + str(i)] = ica2_results_train[:,i-1]
    test['ica_' + str(i)] = ica2_results_test[:, i-1]

# remove  duplicates - needs to be applied to test too
# train = train.T.drop_duplicates().T
# test = test.T.drop_duplicates().T

y_train = train["y"]
y_mean = np.mean(y_train)
train.drop('y', axis=1, inplace=True)
```

训练模型！

```
X_train, X_valid, y_train, y_valid = train_test_split(
       train, y_train, test_size=0.2, random_state=9127)

# create dataset for lightgbm
lgb_train = lgb.Dataset(X_train, y_train)
lgb_valid = lgb.Dataset(X_valid, y_valid, reference=lgb_train)

# to record eval results for plotting
evals_result = {} 

# The r2 is:  0.648019302812 the rmse is: 7.2525692268
# specify your configurations as a dict
params = {
    'task': 'train',
    'boosting_type': 'gbdt',
    'objective': 'regression',
    'metric': {'l2'},
    'num_leaves': 5,
    'learning_rate': 0.06,
    'max_depth': 4,
    'subsample': 0.95,
    'feature_fraction': 0.9,
    'bagging_fraction': 0.85,
    'bagging_freq': 4,
    'min_data_in_leaf':4,
    'min_sum_hessian_in_leaf': 0.8,
    'verbose':10
}

print('Start training...')

# train
gbm = lgb.train(params,
                lgb_train,
                num_boost_round=8000, # 200
                valid_sets=[lgb_train, lgb_valid],
                evals_result=evals_result,
                verbose_eval=10,
                early_stopping_rounds=50) # 50

#print('\nSave model...')
# save model to file
#gbm.save_model('model.txt')

print('Start predicting...')
# predict
y_pred = gbm.predict(X_valid, num_iteration=gbm.best_iteration)
```

估价

```
# feature importances
print('Feature importances:', list(gbm.feature_importance()))

# -------------------------------------------------------
print('Plot metrics during training...')
ax = lgb.plot_metric(evals_result, metric='l2')
plt.show()

print('Plot feature importances...')
ax = lgb.plot_importance(gbm, max_num_features=10)
plt.show()
# -------------------------------------------------------
# eval r2-score 
from sklearn.metrics import r2_score
r2 = r2_score(y_valid, y_pred)

# eval rmse (lower is better)
print('\nThe r2 is: ',r2, 'the rmse is:', mean_squared_error(y_valid, y_pred) ** 0.5)

# -------------------------------------------------------
print('\nPredicting test set...')
y_pred = gbm.predict(test, num_iteration=gbm.best_iteration)

# y_pred = model.predict(dtest)
output = pd.DataFrame({'id': test['ID'], 'y': y_pred})
output.to_csv('submit-lightgbm-ICA-PCA.csv', index=False)

# -----------------------------------------------------------------------------
print("Finished.")
```

这应该对你有用！干杯！

如果你卡住了，一定要伸出手来评论！

其他可能感兴趣的文章:
-[Apache Spark 入门— I | by Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-i-5fbbe7b47667)
-[Apache Spark 入门 II | by Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-ii-fffeab9f5df7)
-[Apache Spark 入门 III | by Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-iii-1758581d87f3)
-[Streamlit 和帕尔默企鹅。上周在网飞的 Binged 非典型… |作者 Sam | Geek Culture | Medium](/geekculture/streamlit-and-palmer-penguins-92a09004ed45)
-[Streamlit 入门。使用 Streamlit 解释你的 EDA 和… | by Sam | Geek Culture | Medium](/geekculture/getting-started-with-streamlit-ed81eafcb298)

干杯，请关注更多此类内容！:)

如果你喜欢它的内容，你现在也可以给我买一杯咖啡！
[samunderscore12 正在创作数据科学内容！(buymeacoffee.com)](https://www.buymeacoffee.com/samunderscore12)