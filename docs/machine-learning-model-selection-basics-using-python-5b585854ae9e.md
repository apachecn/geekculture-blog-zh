# 使用 Python 的机器学习模型选择基础

> 原文：<https://medium.com/geekculture/machine-learning-model-selection-basics-using-python-5b585854ae9e?source=collection_archive---------10----------------------->

![](img/bf925ca6eea91915aa02791f17314773.png)

选择最佳模型是任何数据科学项目的关键步骤。如果你和我一样，愿意为了更好的模型解释而牺牲一点点准确性，那么这篇文章就是为你准备的。就我个人而言，当我在任何数据集上运行模型选择时，我总是想知道手动选择模型(即手动选择最相关的变量)是否会增加任何价值，而不是仅仅使用惩罚模型并让模型自己决定。在本教程中，我们将尝试探索这些问题，并尝试使用手动(子集选择)模型选择技术和正则化来找到房屋数据集的最佳`linear model`以预测房价，并查看哪种方法效果最佳。我们开始吧！！

## 模型选择的目标

简而言之，我们运行模型选择以变得更好`prediction accuracy`，并确保我们的模型是`interpretable`。拥有不必要的非预测性预测器不仅会让你的模型增加`noise`，还会让你的模型变得`uninterpretable`。

## 数据

我们将使用 Kaggle 上的高维数据集房屋数据。让我们导入所需的库，并尝试理解手头的数据。

```
# Data Wrangling
import pandas as pd
import numpy as np

# Plotting
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns

# Statistics
import statsmodels.formula.api as smf
import statsmodels.api as sm

# Machine Learning
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import RidgeCV, Ridge
from sklearn.model_selection import KFold

## Jupyter options
plt.rcParams['figure.figsize'] = [10, 8]housing = pd.read_csv('train.csv')housing.shape(1460, 81)
```

幸运的是，我们的 n(记录数)比 p(预测数)多得多。这是个好兆头。然而，处理一个模型中的 81 个变量本身就是一个挑战。让我们更仔细地看看数据。

```
housing.columnsIndex(['Id', 'MSSubClass', 'MSZoning', 'LotFrontage', 'LotArea', 'Street',
       'Alley', 'LotShape', 'LandContour', 'Utilities', 'LotConfig',
       'LandSlope', 'Neighborhood', 'Condition1', 'Condition2', 'BldgType',
       'HouseStyle', 'OverallQual', 'OverallCond', 'YearBuilt', 'YearRemodAdd',
       'RoofStyle', 'RoofMatl', 'Exterior1st', 'Exterior2nd', 'MasVnrType',
       'MasVnrArea', 'ExterQual', 'ExterCond', 'Foundation', 'BsmtQual',
       'BsmtCond', 'BsmtExposure', 'BsmtFinType1', 'BsmtFinSF1',
       'BsmtFinType2', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'Heating',
       'HeatingQC', 'CentralAir', 'Electrical', '1stFlrSF', '2ndFlrSF',
       'LowQualFinSF', 'GrLivArea', 'BsmtFullBath', 'BsmtHalfBath', 'FullBath',
       'HalfBath', 'BedroomAbvGr', 'KitchenAbvGr', 'KitchenQual',
       'TotRmsAbvGrd', 'Functional', 'Fireplaces', 'FireplaceQu', 'GarageType',
       'GarageYrBlt', 'GarageFinish', 'GarageCars', 'GarageArea', 'GarageQual',
       'GarageCond', 'PavedDrive', 'WoodDeckSF', 'OpenPorchSF',
       'EnclosedPorch', '3SsnPorch', 'ScreenPorch', 'PoolArea', 'PoolQC',
       'Fence', 'MiscFeature', 'MiscVal', 'MoSold', 'YrSold', 'SaleType',
       'SaleCondition', 'SalePrice'],
      dtype='object')
```

你可以在这里得到上述所有变量[的描述。](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data?select=data_description.txt)

## 思念

我注意到一些变量有很高的遗漏，我们需要在拟合任何模型之前修复它们。

```
housing_na = (housing.isnull().sum() / len(housing)) * 100
housing_na = housing_na.drop(housing_na[housing_na == 0].index).sort_values(ascending=False)[:30]
missing_data = pd.DataFrame({'Missing Ratio' :housing_na})
missing_data.head(20).style.bar()
```

![](img/cd433010ced91b2d95bd9e423b1b29b0.png)

我们看到有 6 个变量丢失了超过 15%的记录。为了简单起见，我们可以从分析中删除这些变量，或者用-99 来估算它们，让模型来处理它们。

```
housing = housing.fillna(-99)
```

让我们运行一个简单的相关矩阵来检查我们是否有多重共线性问题。

```
corr_Matrix = housing.corr(method = 'pearson')
sns.heatmap(corr_Matrix)<AxesSubplot:>
```

![](img/f16f977d557b4874c13a251e06e5fc4b.png)

我们看到有许多相互关联的预测因子(这不是一个好现象)。我们将在型号选择:D 中解决这个问题

## 子集选择建模

```
# removing the dependent variable
X = housing.drop(["SalePrice"], axis = 1)
# adding constant required for OLS models
X = sm.add_constant(X)
# dependent variable
y = housing['SalePrice']
# one hot encoding
X = pd.get_dummies(X)
# splitting data into test and train datasets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)X.head()
```

![](img/db91c87aa6771c2f07af738248f27098.png)

让我们用所有变量来拟合一个简单的 OLS 模型。

```
model = sm.OLS(y_train, X_train, hasconst=True).fit()
predict = model.predict(X_train)full_model= model.summary()print(full_model)OLS Regression Results                            
==============================================================================
Dep. Variable:              SalePrice   R-squared:                       0.938
Model:                            OLS   Adj. R-squared:                  0.918
Method:                 Least Squares   F-statistic:                     45.73
Date:                Sat, 04 Dec 2021   Prob (F-statistic):               0.00
Time:                        12:41:45   Log-Likelihood:                -11018.
No. Observations:                 978   AIC:                         2.253e+04
Df Residuals:                     733   BIC:                         2.372e+04
Df Model:                         244                                         
Covariance Type:            nonrobust                                         
=========================================================================================
                            coef    std err          t      P>|t|      [0.025      0.975]
-----------------------------------------------------------------------------------------
const                 -8.577e+04   1.24e+05     -0.692      0.489   -3.29e+05    1.57e+05
Id                        0.0161      1.909      0.008      0.993      -3.732       3.764
MSSubClass              219.7079    133.335      1.648      0.100     -42.056     481.472
LotFrontage              -0.0048     13.376     -0.000      1.000     -26.265      26.255
LotArea                   0.7784      0.147      5.306      0.000       0.490       1.066
OverallQual            7395.5695   1282.933      5.765      0.000    4876.909    9914.230
OverallCond            4634.9259   1102.332      4.205      0.000    2470.822    6799.030
YearBuilt               287.1320     95.410      3.009      0.003      99.822     474.442
YearRemodAdd            136.3689     71.247      1.914      0.056      -3.505     276.242
MasVnrArea               20.4746      7.606      2.692      0.007       5.542      35.407
BsmtFinSF1               13.4053      3.744      3.580      0.000       6.054      20.756
BsmtFinSF2                6.8500      7.703      0.889      0.374      -8.273      21.973
BsmtUnfSF                -0.1708      3.481     -0.049      0.961      -7.005       6.664
TotalBsmtSF              20.0845      5.020      4.001      0.000      10.230      29.939
1stFlrSF                  7.4099      9.166      0.808      0.419     -10.586      25.406
2ndFlrSF                 25.2301      7.742      3.259      0.001      10.030      40.430
LowQualFinSF             -6.4070     21.555     -0.297      0.766     -48.725      35.911
GrLivArea                26.2331      8.362      3.137      0.002       9.816      42.650
BsmtFullBath           3682.6637   2473.072      1.489      0.137   -1172.485    8537.812
BsmtHalfBath            679.4655   3725.577      0.182      0.855   -6634.609    7993.540
FullBath               4945.7546   2702.233      1.830      0.068    -359.284    1.03e+04
HalfBath               2372.1613   2648.941      0.896      0.371   -2828.255    7572.577
BedroomAbvGr          -3739.1659   1756.499     -2.129      0.034   -7187.535    -290.797
KitchenAbvGr          -1.747e+04   8019.813     -2.178      0.030   -3.32e+04   -1721.941
TotRmsAbvGrd           2603.2188   1228.757      2.119      0.034     190.916    5015.521
Fireplaces             4024.4404   3168.655      1.270      0.204   -2196.282    1.02e+04
GarageYrBlt             -88.5745     73.503     -1.205      0.229    -232.876      55.727
GarageCars             1430.8822   2784.843      0.514      0.608   -4036.337    6898.102
GarageArea               38.3408      9.670      3.965      0.000      19.357      57.325
WoodDeckSF               15.9321      7.204      2.211      0.027       1.788      30.076
OpenPorchSF              -5.3409     13.299     -0.402      0.688     -31.450      20.768
EnclosedPorch            15.0364     16.037      0.938      0.349     -16.448      46.521
3SsnPorch                46.9285     23.881      1.965      0.050       0.046      93.811
ScreenPorch              51.3760     15.575      3.299      0.001      20.798      81.954
PoolArea                385.3757    150.153      2.567      0.010      90.595     680.156
MiscVal                   7.2691      8.061      0.902      0.368      -8.557      23.095
MoSold                 -408.3528    305.933     -1.335      0.182   -1008.961     192.256
YrSold                 -117.2414    635.071     -0.185      0.854   -1364.016    1129.533
MSZoning_C (all)      -3.141e+04   2.82e+04     -1.115      0.265   -8.67e+04    2.39e+04
MSZoning_FV           -5675.1716    2.6e+04     -0.218      0.828   -5.68e+04    4.54e+04
MSZoning_RH           -2.192e+04   2.54e+04     -0.863      0.388   -7.18e+04    2.79e+04
MSZoning_RL           -1.536e+04   2.47e+04     -0.622      0.534   -6.39e+04    3.32e+04
MSZoning_RM           -1.141e+04   2.52e+04     -0.452      0.651   -6.09e+04    3.81e+04
Street_Grvl           -7.469e+04   6.32e+04     -1.182      0.238   -1.99e+05    4.94e+04
Street_Pave           -1.109e+04   6.22e+04     -0.178      0.859   -1.33e+05    1.11e+05
Alley_-99             -3.355e+04   4.14e+04     -0.811      0.418   -1.15e+05    4.77e+04
Alley_Grvl            -3.106e+04   4.14e+04     -0.750      0.454   -1.12e+05    5.02e+04
Alley_Pave            -2.116e+04   4.16e+04     -0.508      0.611   -1.03e+05    6.06e+04
LotShape_IR1          -2.483e+04   3.11e+04     -0.798      0.425   -8.59e+04    3.62e+04
LotShape_IR2          -2.124e+04   3.13e+04     -0.679      0.498   -8.27e+04    4.02e+04
LotShape_IR3          -1.686e+04   3.19e+04     -0.529      0.597   -7.95e+04    4.57e+04
LotShape_Reg          -2.284e+04   3.12e+04     -0.733      0.464    -8.4e+04    3.83e+04
LandContour_Bnk       -2.567e+04   3.15e+04     -0.815      0.415   -8.75e+04    3.61e+04
LandContour_HLS       -7047.2079   3.11e+04     -0.227      0.821   -6.81e+04     5.4e+04
LandContour_Low        -3.48e+04   3.13e+04     -1.110      0.267   -9.63e+04    2.67e+04
LandContour_Lvl       -1.826e+04   3.11e+04     -0.586      0.558   -7.94e+04    4.29e+04
Utilities_AllPub      -2.862e+04   6.37e+04     -0.449      0.654   -1.54e+05    9.65e+04
Utilities_NoSeWa      -5.715e+04   6.32e+04     -0.904      0.366   -1.81e+05     6.7e+04
LotConfig_Corner      -1.291e+04    2.5e+04     -0.516      0.606    -6.2e+04    3.62e+04
LotConfig_CulDSac     -4231.2624   2.54e+04     -0.167      0.868   -5.41e+04    4.56e+04
LotConfig_FR2         -1.983e+04   2.54e+04     -0.781      0.435   -6.97e+04       3e+04
LotConfig_FR3         -3.542e+04   2.72e+04     -1.303      0.193   -8.88e+04    1.79e+04
LotConfig_Inside      -1.338e+04    2.5e+04     -0.534      0.593   -6.25e+04    3.58e+04
LandSlope_Gtl         -1.592e+04   4.17e+04     -0.382      0.703   -9.78e+04     6.6e+04
LandSlope_Mod         -1.069e+04   4.16e+04     -0.257      0.797   -9.24e+04     7.1e+04
LandSlope_Sev         -5.916e+04   4.32e+04     -1.370      0.171   -1.44e+05    2.56e+04
Neighborhood_Blmngtn   7380.0979   9678.463      0.763      0.446   -1.16e+04    2.64e+04
Neighborhood_Blueste  -3274.8942   2.33e+04     -0.141      0.888    -4.9e+04    4.24e+04
Neighborhood_BrDale    -347.7544   1.04e+04     -0.033      0.973   -2.07e+04    2.01e+04
Neighborhood_BrkSide  -9498.7577   7484.389     -1.269      0.205   -2.42e+04    5194.637
Neighborhood_ClearCr  -9890.1157   8861.146     -1.116      0.265   -2.73e+04    7506.135
Neighborhood_CollgCr  -7065.0221   6299.646     -1.121      0.262   -1.94e+04    5302.477
Neighborhood_Crawfor   6420.0063   7124.550      0.901      0.368   -7566.951    2.04e+04
Neighborhood_Edwards  -2.254e+04   6304.074     -3.575      0.000   -3.49e+04   -1.02e+04
Neighborhood_Gilbert  -7889.0876   7039.164     -1.121      0.263   -2.17e+04    5930.238
Neighborhood_IDOTRR   -2.397e+04   9034.729     -2.653      0.008   -4.17e+04   -6235.677
Neighborhood_MeadowV  -1.159e+04   1.14e+04     -1.020      0.308   -3.39e+04    1.07e+04
Neighborhood_Mitchel  -2.032e+04   6878.456     -2.955      0.003   -3.38e+04   -6819.115
Neighborhood_NAmes    -1.779e+04   5993.031     -2.969      0.003   -2.96e+04   -6026.023
Neighborhood_NPkVill   2.004e+04   1.74e+04      1.149      0.251   -1.42e+04    5.43e+04
Neighborhood_NWAmes   -1.655e+04   6483.000     -2.553      0.011   -2.93e+04   -3824.774
Neighborhood_NoRidge   2.298e+04   7901.745      2.909      0.004    7471.376    3.85e+04
Neighborhood_NridgHt   2.117e+04   7038.716      3.007      0.003    7348.497     3.5e+04
Neighborhood_OldTown  -2.255e+04   7853.861     -2.871      0.004    -3.8e+04   -7128.269
Neighborhood_SWISU    -6865.9161   9843.421     -0.698      0.486   -2.62e+04    1.25e+04
Neighborhood_Sawyer   -7769.1685   6554.031     -1.185      0.236   -2.06e+04    5097.741
Neighborhood_SawyerW  -5695.4938   7266.711     -0.784      0.433      -2e+04    8570.554
Neighborhood_Somerst  -4864.8313   8937.547     -0.544      0.586   -2.24e+04    1.27e+04
Neighborhood_StoneBr   4.114e+04   8654.297      4.754      0.000    2.42e+04    5.81e+04
Neighborhood_Timber   -7869.0040   7859.467     -1.001      0.317   -2.33e+04    7560.747
Neighborhood_Veenker   1435.4807   1.04e+04      0.137      0.891   -1.91e+04    2.19e+04
Condition1_Artery     -1.765e+04    1.5e+04     -1.177      0.239   -4.71e+04    1.18e+04
Condition1_Feedr      -9060.0127   1.46e+04     -0.622      0.534   -3.76e+04    1.95e+04
Condition1_Norm       -2986.3983   1.42e+04     -0.211      0.833   -3.08e+04    2.49e+04
Condition1_PosA       -1.679e+04   1.76e+04     -0.953      0.341   -5.14e+04    1.78e+04
Condition1_PosN        2616.1093   1.65e+04      0.158      0.874   -2.98e+04     3.5e+04
Condition1_RRAe       -2.934e+04   1.62e+04     -1.813      0.070   -6.11e+04    2432.873
Condition1_RRAn       -5297.1419   1.54e+04     -0.345      0.730   -3.55e+04    2.49e+04
Condition1_RRNe       -4805.1918   2.44e+04     -0.197      0.844   -5.28e+04    4.32e+04
Condition1_RRNn       -2454.5148    1.9e+04     -0.129      0.897   -3.97e+04    3.48e+04
Condition2_Artery      5.025e+04   2.95e+04      1.704      0.089   -7631.520    1.08e+05
Condition2_Feedr       3.916e+04   2.74e+04      1.429      0.154   -1.47e+04     9.3e+04
Condition2_Norm        3.471e+04   2.26e+04      1.534      0.125   -9712.361    7.91e+04
Condition2_PosA       -2.208e-11   6.81e-11     -0.324      0.746   -1.56e-10    1.12e-10
Condition2_PosN       -1.906e+05   2.74e+04     -6.950      0.000   -2.44e+05   -1.37e+05
Condition2_RRAe       -5.153e+04   3.65e+04     -1.413      0.158   -1.23e+05       2e+04
Condition2_RRAn        3.221e+04   2.94e+04      1.095      0.274   -2.56e+04       9e+04
Condition2_RRNn       -7.421e-10   1.31e-10     -5.664      0.000   -9.99e-10   -4.85e-10
BldgType_1Fam          1.545e+04   2.67e+04      0.579      0.563   -3.69e+04    6.78e+04
BldgType_2fmCon       -1.994e+04   2.68e+04     -0.744      0.457   -7.26e+04    3.27e+04
BldgType_Duplex       -7608.7972   2.57e+04     -0.296      0.767    -5.8e+04    4.28e+04
BldgType_Twnhs        -3.841e+04   2.57e+04     -1.493      0.136   -8.89e+04    1.21e+04
BldgType_TwnhsE       -3.526e+04   2.56e+04     -1.377      0.169   -8.55e+04     1.5e+04
HouseStyle_1.5Fin     -5122.2422    1.6e+04     -0.320      0.749   -3.65e+04    2.63e+04
HouseStyle_1.5Unf      7827.7395   1.77e+04      0.442      0.659   -2.69e+04    4.26e+04
HouseStyle_1Story      5034.9944   1.66e+04      0.303      0.762   -2.75e+04    3.76e+04
HouseStyle_2.5Fin     -2.496e+04   2.13e+04     -1.173      0.241   -6.67e+04    1.68e+04
HouseStyle_2.5Unf     -1.306e+04   1.79e+04     -0.731      0.465   -4.81e+04     2.2e+04
HouseStyle_2Story     -1.637e+04    1.6e+04     -1.020      0.308   -4.79e+04    1.51e+04
HouseStyle_SFoyer     -2.131e+04   1.72e+04     -1.242      0.215    -5.5e+04    1.24e+04
HouseStyle_SLvl       -1.782e+04   1.68e+04     -1.058      0.290   -5.09e+04    1.52e+04
RoofStyle_Flat        -2.901e+04   3.01e+04     -0.963      0.336   -8.82e+04    3.02e+04
RoofStyle_Gable       -3.144e+04    2.2e+04     -1.431      0.153   -7.46e+04    1.17e+04
RoofStyle_Gambrel     -2.779e+04   2.33e+04     -1.193      0.233   -7.35e+04     1.8e+04
RoofStyle_Hip         -3.109e+04    2.2e+04     -1.414      0.158   -7.42e+04    1.21e+04
RoofStyle_Mansard     -1833.0660   2.58e+04     -0.071      0.943   -5.24e+04    4.87e+04
RoofStyle_Shed         3.539e+04   3.96e+04      0.893      0.372   -4.24e+04    1.13e+05
RoofMatl_ClyTile       -5.04e+05   6.05e+04     -8.335      0.000   -6.23e+05   -3.85e+05
RoofMatl_CompShg       5.908e+04   1.71e+04      3.462      0.001    2.56e+04    9.26e+04
RoofMatl_Membran       2.728e-10   1.74e-10      1.571      0.117    -6.8e-11    6.14e-10
RoofMatl_Metal         1.072e+05    3.5e+04      3.059      0.002    3.84e+04    1.76e+05
RoofMatl_Roll          5.965e+04   2.81e+04      2.125      0.034    4541.633    1.15e+05
RoofMatl_Tar&Grv       5.875e+04   2.64e+04      2.223      0.026    6873.758    1.11e+05
RoofMatl_WdShake        5.84e+04   2.61e+04      2.241      0.025    7233.559     1.1e+05
RoofMatl_WdShngl       7.518e+04   2.36e+04      3.190      0.001    2.89e+04    1.21e+05
Exterior1st_AsbShng     143.0780   1.65e+04      0.009      0.993   -3.22e+04    3.25e+04
Exterior1st_AsphShn   -4.197e+04   3.74e+04     -1.123      0.262   -1.15e+05    3.14e+04
Exterior1st_BrkComm    4.979e+04   3.58e+04      1.393      0.164   -2.04e+04     1.2e+05
Exterior1st_BrkFace    1.209e+04   1.15e+04      1.050      0.294   -1.05e+04    3.47e+04
Exterior1st_CBlock    -1.521e+04   1.62e+04     -0.940      0.348    -4.7e+04    1.66e+04
Exterior1st_CemntBd   -1.827e+04   1.73e+04     -1.056      0.291   -5.22e+04    1.57e+04
Exterior1st_HdBoard   -7091.2586   1.14e+04     -0.624      0.533   -2.94e+04    1.52e+04
Exterior1st_ImStucc    4.204e-10   7.03e-11      5.976      0.000    2.82e-10    5.58e-10
Exterior1st_MetalSd   -7316.6913   1.44e+04     -0.508      0.611   -3.56e+04    2.09e+04
Exterior1st_Plywood   -5214.3518   1.14e+04     -0.456      0.649   -2.77e+04    1.73e+04
Exterior1st_Stone      6805.9727   3.19e+04      0.213      0.831   -5.58e+04    6.94e+04
Exterior1st_Stucco     -1.26e+04   1.38e+04     -0.913      0.362   -3.97e+04    1.45e+04
Exterior1st_VinylSd   -1.355e+04    1.3e+04     -1.046      0.296    -3.9e+04    1.19e+04
Exterior1st_Wd Sdng   -1.806e+04    1.1e+04     -1.640      0.102   -3.97e+04    3564.140
Exterior1st_WdShing   -1.533e+04   1.23e+04     -1.247      0.213   -3.95e+04    8802.525
Exterior2nd_AsbShng   -6751.2397    1.5e+04     -0.450      0.652   -3.62e+04    2.27e+04
Exterior2nd_AsphShn    1.948e+04   2.71e+04      0.719      0.472   -3.37e+04    7.26e+04
Exterior2nd_Brk Cmn   -1.892e+04   2.13e+04     -0.889      0.374   -6.07e+04    2.29e+04
Exterior2nd_BrkFace   -2708.8839   1.25e+04     -0.217      0.828   -2.72e+04    2.18e+04
Exterior2nd_CBlock    -1.521e+04   1.62e+04     -0.940      0.348    -4.7e+04    1.66e+04
Exterior2nd_CmentBd    1.049e+04   1.73e+04      0.606      0.545   -2.35e+04    4.45e+04
Exterior2nd_HdBoard   -8237.2283   9977.522     -0.826      0.409   -2.78e+04    1.14e+04
Exterior2nd_ImStucc   -2963.4745    1.6e+04     -0.186      0.853   -3.43e+04    2.84e+04
Exterior2nd_MetalSd   -4804.1671   1.32e+04     -0.363      0.717   -3.08e+04    2.12e+04
Exterior2nd_Other     -3.486e+04   2.45e+04     -1.423      0.155    -8.3e+04    1.32e+04
Exterior2nd_Plywood   -1.293e+04   9886.768     -1.308      0.191   -3.23e+04    6475.216
Exterior2nd_Stone     -1.674e+04   2.01e+04     -0.832      0.406   -5.63e+04    2.28e+04
Exterior2nd_Stucco     9291.7757   1.27e+04      0.731      0.465   -1.57e+04    3.43e+04
Exterior2nd_VinylSd   -2765.2698    1.1e+04     -0.250      0.802   -2.44e+04    1.89e+04
Exterior2nd_Wd Sdng    6117.7839   9695.753      0.631      0.528   -1.29e+04    2.52e+04
Exterior2nd_Wd Shng   -4251.4593   1.04e+04     -0.407      0.684   -2.48e+04    1.63e+04
MasVnrType_-99        -1.391e+04   2.84e+04     -0.491      0.624   -6.96e+04    4.18e+04
MasVnrType_BrkCmn     -2.529e+04   2.58e+04     -0.981      0.327   -7.59e+04    2.53e+04
MasVnrType_BrkFace    -1.809e+04   2.49e+04     -0.726      0.468    -6.7e+04    3.08e+04
MasVnrType_None       -1.517e+04   2.48e+04     -0.612      0.541   -6.38e+04    3.35e+04
MasVnrType_Stone      -1.331e+04   2.54e+04     -0.525      0.600   -6.31e+04    3.65e+04
ExterQual_Ex          -1.073e+04   3.18e+04     -0.338      0.735   -7.31e+04    5.16e+04
ExterQual_Fa          -1.056e+04   3.25e+04     -0.325      0.745   -7.43e+04    5.32e+04
ExterQual_Gd           -3.23e+04   3.12e+04     -1.037      0.300   -9.35e+04    2.89e+04
ExterQual_TA          -3.218e+04   3.12e+04     -1.032      0.303   -9.34e+04    2.91e+04
ExterCond_Ex           -1.49e+04    2.9e+04     -0.513      0.608   -7.19e+04    4.21e+04
ExterCond_Fa          -2.048e+04   2.59e+04     -0.792      0.428   -7.12e+04    3.03e+04
ExterCond_Gd          -2.211e+04   2.54e+04     -0.869      0.385   -7.21e+04    2.78e+04
ExterCond_Po          -7799.1824    3.4e+04     -0.230      0.818   -7.45e+04    5.89e+04
ExterCond_TA          -2.048e+04   2.52e+04     -0.812      0.417      -7e+04     2.9e+04
Foundation_BrkTil     -1.261e+04    2.1e+04     -0.600      0.549   -5.39e+04    2.86e+04
Foundation_CBlock     -6143.7862   2.11e+04     -0.291      0.771   -4.75e+04    3.53e+04
Foundation_PConc      -3834.2469   2.11e+04     -0.182      0.856   -4.52e+04    3.76e+04
Foundation_Slab       -2.579e+04   2.31e+04     -1.117      0.265   -7.11e+04    1.96e+04
Foundation_Stone      -2850.8484   2.45e+04     -0.116      0.907    -5.1e+04    4.53e+04
Foundation_Wood       -3.454e+04   2.46e+04     -1.404      0.161   -8.28e+04    1.38e+04
BsmtQual_-99          -1.496e+04   2.23e+04     -0.672      0.502   -5.86e+04    2.87e+04
BsmtQual_Ex           -2166.4681    2.6e+04     -0.083      0.934   -5.33e+04     4.9e+04
BsmtQual_Fa           -1.857e+04   2.56e+04     -0.727      0.468   -6.87e+04    3.16e+04
BsmtQual_Gd           -2.486e+04   2.56e+04     -0.970      0.333   -7.52e+04    2.55e+04
BsmtQual_TA           -2.521e+04   2.55e+04     -0.988      0.323   -7.53e+04    2.49e+04
BsmtCond_-99          -1.496e+04   2.23e+04     -0.672      0.502   -5.86e+04    2.87e+04
BsmtCond_Fa           -3.248e+04   2.63e+04     -1.233      0.218   -8.42e+04    1.92e+04
BsmtCond_Gd           -3.105e+04   2.64e+04     -1.177      0.240   -8.28e+04    2.07e+04
BsmtCond_Po            1.716e+04   3.87e+04      0.444      0.657   -5.87e+04    9.31e+04
BsmtCond_TA           -2.444e+04   2.66e+04     -0.920      0.358   -7.66e+04    2.77e+04
BsmtExposure_-99      -1.496e+04   2.23e+04     -0.672      0.502   -5.86e+04    2.87e+04
BsmtExposure_Av        -1.98e+04   2.55e+04     -0.777      0.438   -6.99e+04    3.03e+04
BsmtExposure_Gd       -2772.8091   2.55e+04     -0.109      0.913   -5.28e+04    4.72e+04
BsmtExposure_Mn       -2.406e+04   2.56e+04     -0.939      0.348   -7.44e+04    2.63e+04
BsmtExposure_No       -2.418e+04   2.56e+04     -0.944      0.346   -7.45e+04    2.61e+04
BsmtFinType1_-99      -1.496e+04   2.23e+04     -0.672      0.502   -5.86e+04    2.87e+04
BsmtFinType1_ALQ      -1.274e+04   1.71e+04     -0.747      0.455   -4.62e+04    2.07e+04
BsmtFinType1_BLQ      -1.194e+04   1.72e+04     -0.694      0.488   -4.57e+04    2.18e+04
BsmtFinType1_GLQ      -8396.1457   1.73e+04     -0.485      0.628   -4.24e+04    2.56e+04
BsmtFinType1_LwQ      -1.488e+04   1.73e+04     -0.863      0.389   -4.88e+04     1.9e+04
BsmtFinType1_Rec      -9788.7767    1.7e+04     -0.575      0.566   -4.32e+04    2.37e+04
BsmtFinType1_Unf      -1.306e+04   1.72e+04     -0.761      0.447   -4.68e+04    2.06e+04
BsmtFinType2_-99      -1.496e+04   2.23e+04     -0.672      0.502   -5.86e+04    2.87e+04
BsmtFinType2_ALQ      -4651.3042   1.79e+04     -0.260      0.795   -3.97e+04    3.04e+04
BsmtFinType2_BLQ      -2.047e+04   1.81e+04     -1.133      0.258   -5.59e+04     1.5e+04
BsmtFinType2_GLQ      -7872.5820   1.93e+04     -0.408      0.683   -4.57e+04       3e+04
BsmtFinType2_LwQ      -1.372e+04   1.76e+04     -0.780      0.436   -4.83e+04    2.08e+04
BsmtFinType2_Rec      -1.117e+04   1.74e+04     -0.641      0.522   -4.54e+04     2.3e+04
BsmtFinType2_Unf      -1.293e+04   1.76e+04     -0.733      0.464   -4.76e+04    2.17e+04
Heating_Floor         -1.014e+04   3.09e+04     -0.328      0.743   -7.09e+04    5.06e+04
Heating_GasA          -5543.8246   2.16e+04     -0.256      0.798    -4.8e+04    3.69e+04
Heating_GasW          -2.307e+04   2.22e+04     -1.039      0.299   -6.67e+04    2.05e+04
Heating_Grav          -1.932e+04    2.5e+04     -0.773      0.440   -6.84e+04    2.97e+04
Heating_OthW          -3.731e+04    2.7e+04     -1.382      0.167   -9.03e+04    1.57e+04
Heating_Wall           9606.3156   2.67e+04      0.360      0.719   -4.28e+04    6.21e+04
HeatingQC_Ex          -1.655e+04   2.54e+04     -0.651      0.515   -6.64e+04    3.33e+04
HeatingQC_Fa           -1.67e+04   2.57e+04     -0.650      0.516   -6.72e+04    3.38e+04
HeatingQC_Gd          -2.116e+04   2.52e+04     -0.840      0.401   -7.06e+04    2.83e+04
HeatingQC_Po          -1.178e+04   3.33e+04     -0.354      0.724   -7.71e+04    5.36e+04
HeatingQC_TA          -1.959e+04   2.54e+04     -0.771      0.441   -6.95e+04    3.03e+04
CentralAir_N          -4.296e+04    6.2e+04     -0.693      0.489   -1.65e+05    7.88e+04
CentralAir_Y          -4.282e+04    6.2e+04     -0.691      0.490   -1.65e+05    7.89e+04
Electrical_-99         3288.0964   3.16e+04      0.104      0.917   -5.88e+04    6.54e+04
Electrical_FuseA       -1.78e+04   2.55e+04     -0.699      0.485   -6.78e+04    3.22e+04
Electrical_FuseF      -2.222e+04   2.59e+04     -0.857      0.392   -7.31e+04    2.87e+04
Electrical_FuseP      -3.134e+04   3.08e+04     -1.017      0.310   -9.18e+04    2.92e+04
Electrical_Mix         5.113e-12   1.01e-11      0.506      0.613   -1.47e-11    2.49e-11
Electrical_SBrkr      -1.769e+04   2.56e+04     -0.690      0.490    -6.8e+04    3.26e+04
KitchenQual_Ex        -9547.6947   3.15e+04     -0.303      0.762   -7.13e+04    5.22e+04
KitchenQual_Fa        -2.042e+04    3.1e+04     -0.659      0.510   -8.13e+04    4.05e+04
KitchenQual_Gd        -2.751e+04   3.12e+04     -0.883      0.377   -8.87e+04    3.36e+04
KitchenQual_TA        -2.829e+04   3.11e+04     -0.911      0.363   -8.92e+04    3.27e+04
Functional_Maj1       -3834.6393   1.94e+04     -0.198      0.843   -4.19e+04    3.42e+04
Functional_Maj2       -1.841e+04   2.27e+04     -0.812      0.417   -6.29e+04    2.61e+04
Functional_Min1       -9748.8043   1.88e+04     -0.520      0.604   -4.66e+04    2.71e+04
Functional_Min2        2121.8148   1.95e+04      0.109      0.914   -3.62e+04    4.05e+04
Functional_Mod        -2226.9105   2.03e+04     -0.110      0.913   -4.21e+04    3.77e+04
Functional_Sev        -6.004e+04   3.14e+04     -1.914      0.056   -1.22e+05    1548.005
Functional_Typ         6367.1997   1.85e+04      0.345      0.730   -2.99e+04    4.26e+04
FireplaceQu_-99       -1.276e+04    2.1e+04     -0.607      0.544    -5.4e+04    2.85e+04
FireplaceQu_Ex        -1.753e+04   2.18e+04     -0.804      0.421   -6.03e+04    2.52e+04
FireplaceQu_Fa        -1.764e+04   2.12e+04     -0.833      0.405   -5.92e+04    2.39e+04
FireplaceQu_Gd        -1.601e+04   2.07e+04     -0.772      0.440   -5.67e+04    2.47e+04
FireplaceQu_Po        -9709.2683   2.11e+04     -0.461      0.645   -5.11e+04    3.17e+04
FireplaceQu_TA        -1.212e+04   2.08e+04     -0.582      0.560    -5.3e+04    2.87e+04
GarageType_-99        -4.948e+04   3.77e+04     -1.314      0.189   -1.23e+05    2.45e+04
GarageType_2Types     -1.856e+04   2.09e+04     -0.887      0.375   -5.96e+04    2.25e+04
GarageType_Attchd     -6918.0980   1.82e+04     -0.379      0.705   -4.27e+04    2.89e+04
GarageType_Basment      344.1662   1.97e+04      0.017      0.986   -3.83e+04     3.9e+04
GarageType_BuiltIn    -7289.8574   1.88e+04     -0.388      0.698   -4.41e+04    2.95e+04
GarageType_CarPort      954.5906   2.09e+04      0.046      0.964   -4.02e+04    4.21e+04
GarageType_Detchd     -4826.4449   1.83e+04     -0.264      0.792   -4.07e+04     3.1e+04
GarageFinish_-99      -4.948e+04   3.77e+04     -1.314      0.189   -1.23e+05    2.45e+04
GarageFinish_Fin      -1.197e+04   3.56e+04     -0.336      0.737   -8.19e+04     5.8e+04
GarageFinish_RFn      -1.363e+04   3.55e+04     -0.383      0.702   -8.34e+04    5.61e+04
GarageFinish_Unf       -1.07e+04   3.55e+04     -0.301      0.763   -8.04e+04     5.9e+04
GarageQual_-99        -4.948e+04   3.77e+04     -1.314      0.189   -1.23e+05    2.45e+04
GarageQual_Ex          1.173e+05    3.7e+04      3.167      0.002    4.46e+04     1.9e+05
GarageQual_Fa         -3.074e+04   2.44e+04     -1.260      0.208   -7.86e+04    1.72e+04
GarageQual_Gd         -2.836e+04   2.57e+04     -1.106      0.269   -7.87e+04     2.2e+04
GarageQual_Po         -6.734e+04   3.87e+04     -1.740      0.082   -1.43e+05    8651.154
GarageQual_TA         -2.713e+04   2.48e+04     -1.096      0.274   -7.57e+04    2.15e+04
GarageCond_-99        -4.948e+04   3.77e+04     -1.314      0.189   -1.23e+05    2.45e+04
GarageCond_Ex          -1.22e+05   3.99e+04     -3.062      0.002      -2e+05   -4.38e+04
GarageCond_Fa          1.407e+04   2.33e+04      0.605      0.545   -3.16e+04    5.97e+04
GarageCond_Gd          1.533e+04    2.4e+04      0.640      0.523   -3.17e+04    6.24e+04
GarageCond_Po          3.871e+04   2.81e+04      1.379      0.168   -1.64e+04    9.38e+04
GarageCond_TA          1.764e+04   2.28e+04      0.773      0.440   -2.72e+04    6.24e+04
PavedDrive_N          -2.749e+04   4.14e+04     -0.664      0.507   -1.09e+05    5.38e+04
PavedDrive_P          -3.041e+04   4.15e+04     -0.733      0.464   -1.12e+05    5.11e+04
PavedDrive_Y          -2.787e+04   4.14e+04     -0.673      0.501   -1.09e+05    5.34e+04
PoolQC_-99             1.331e+05   9.46e+04      1.407      0.160   -5.27e+04    3.19e+05
PoolQC_Ex             -9.412e+04   3.26e+04     -2.888      0.004   -1.58e+05   -3.01e+04
PoolQC_Fa             -4.679e+04   2.59e+04     -1.809      0.071   -9.76e+04    3978.671
PoolQC_Gd               -7.8e+04   1.99e+04     -3.929      0.000   -1.17e+05    -3.9e+04
Fence_-99             -1.501e+04   2.49e+04     -0.604      0.546   -6.38e+04    3.38e+04
Fence_GdPrv           -1.811e+04   2.52e+04     -0.720      0.472   -6.75e+04    3.13e+04
Fence_GdWo             -1.55e+04   2.51e+04     -0.618      0.537   -6.48e+04    3.38e+04
Fence_MnPrv           -1.444e+04   2.51e+04     -0.575      0.565   -6.37e+04    3.48e+04
Fence_MnWw            -2.271e+04   2.56e+04     -0.888      0.375   -7.29e+04    2.75e+04
MiscFeature_-99        2.034e+04   3.37e+04      0.603      0.547   -4.59e+04    8.65e+04
MiscFeature_Gar2      -5.153e+04   3.65e+04     -1.413      0.158   -1.23e+05       2e+04
MiscFeature_Othr      -2.119e+04   3.75e+04     -0.566      0.572   -9.47e+04    5.24e+04
MiscFeature_Shed        1.34e+04    3.1e+04      0.433      0.665   -4.74e+04    7.42e+04
MiscFeature_TenC      -4.679e+04   2.59e+04     -1.809      0.071   -9.76e+04    3978.671
SaleType_COD          -1.648e+04   1.63e+04     -1.008      0.314   -4.86e+04    1.56e+04
SaleType_CWD           1684.4206   1.84e+04      0.092      0.927   -3.44e+04    3.78e+04
SaleType_Con           2.152e+04   2.78e+04      0.774      0.439   -3.31e+04    7.61e+04
SaleType_ConLD        -2790.0953   1.76e+04     -0.159      0.874   -3.73e+04    3.17e+04
SaleType_ConLI        -3.555e+04    2.7e+04     -1.318      0.188   -8.85e+04    1.74e+04
SaleType_ConLw        -6424.3636   2.17e+04     -0.296      0.767    -4.9e+04    3.61e+04
SaleType_New          -1.232e+04   2.69e+04     -0.458      0.647   -6.52e+04    4.05e+04
SaleType_Oth          -1.529e+04   2.63e+04     -0.582      0.561   -6.69e+04    3.63e+04
SaleType_WD           -2.012e+04   1.54e+04     -1.308      0.191   -5.03e+04    1.01e+04
SaleCondition_Abnorml -2.445e+04   2.14e+04     -1.142      0.254   -6.65e+04    1.76e+04
SaleCondition_AdjLand -5747.5605   2.44e+04     -0.236      0.814   -5.36e+04    4.21e+04
SaleCondition_Alloca   -816.1514   2.47e+04     -0.033      0.974   -4.94e+04    4.78e+04
SaleCondition_Family  -2.416e+04   2.19e+04     -1.103      0.270   -6.72e+04    1.88e+04
SaleCondition_Normal  -1.827e+04   2.15e+04     -0.850      0.395   -6.05e+04    2.39e+04
SaleCondition_Partial -1.232e+04   3.01e+04     -0.409      0.682   -7.14e+04    4.68e+04
==============================================================================
Omnibus:                      256.144   Durbin-Watson:                   1.998
Prob(Omnibus):                  0.000   Jarque-Bera (JB):            13099.153
Skew:                           0.277   Prob(JB):                         0.00
Kurtosis:                      20.921   Cond. No.                     2.65e+17
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The smallest eigenvalue is 3.62e-24\. This might indicate that there are
strong multicollinearity problems or that the design matrix is singular.
```

## 一些重要的观察结果:

*   首先要注意的是我们得到了 R = 94(这太棒了！！)这意味着我们能够用手头的协变量来解释大量的方差。换句话说，我们有一套很好的变量来解释和预测房价。
*   很难扫描所有变量(也称为暴力),但我们看到一些 p 值非常小的信号。
*   理想情况下，我们应该运行一个[子集选择](https://en.wikipedia.org/wiki/Feature_selection)，遍历所有可能的变量组合。然而，给定变量的数量(> 300)，在计算上不可能运行子集选择。

## 使用正向模型选择的子集选择

我们将尝试`[forward model selection](https://en.wikipedia.org/wiki/Stepwise_regression)`。前向逐步选择从截距开始，然后逐渐一个接一个地添加预测值，从而最大程度地提高模型的拟合度。这是一个贪婪的算法，产生一个嵌套的模型序列，索引为 k，其中 k 是应该确定的子集大小。然而，这种方法有 300 多个变量，计算量非常大。但是不要担心，我们会看到如何使用测试集方法来解决这个问题，这样我们就不会过度使用大量的变量来运行我们的模型。在实践中，很少有变量是我们真正关心的，以获得一个好的模型。这就是偏差-方差权衡真正发挥重要作用的地方。对于本教程，让我们在最大变量数= 30 的情况下运行正向模型选择，并观察它的行为。我们将根据`[Residual Sum of Squares](https://en.wikipedia.org/wiki/Residual_sum_of_squares)`选择最佳型号

```
# function to extract RSS given a model
def processSubset(feature_set):
    if 'const' not in feature_set:
        feature_set = ['const'] + feature_set
    X_mod = X_train
    X_mod = X_mod[list(feature_set)]
    model = sm.OLS(y_train,X_mod, hasconst=True)
    regr = model.fit()
    RSS_train = ((regr.predict(X_mod) - y_train) ** 2).sum()
    X_mod_tst = sm.add_constant(X_test)
    X_mod_tst = X_mod_tst[list(feature_set)]
    RSS_test = ((regr.predict(X_mod_tst) - y_test) ** 2).sum()
    return {"model":regr, "RSSTrain":RSS_train, "RSSTest":RSS_test, "Features": list(feature_set)}# function that implements forward selection 
def forward(predictors):
    remaining_predictors = [p for p in X_train.columns if p not in predictors]
    tic = time.time()
    results = []
    for p in remaining_predictors:
        if len(predictors+[p]) == 1:
            pass
        results.append(processSubset(predictors+[p]))
    models = pd.DataFrame(results)  
    best_model = models.loc[models['RSSTest'].argmin()]
    toc = time.time()
    print("Processed ", models.shape[0], "models on", len(predictors)+1, "predictors in", (toc-tic), "seconds.")
    return best_modelimport time
n_vars = 30
models_fwd = pd.DataFrame(columns=["RSSTrain", "RSSTest", "model", "Features"])

tic = time.time()
predictors = []

for i in range(1,n_vars+1):  
    models_fwd.loc[i] = forward(predictors)
    predictors = models_fwd.loc[i]["model"].model.exog_names

toc = time.time()
print("Total elapsed time:", (toc-tic), "seconds.")Processed  306 models on 1 predictors in 17.51478409767151 seconds.
Processed  304 models on 3 predictors in 17.56110692024231 seconds.
Processed  303 models on 4 predictors in 17.335726022720337 seconds.
Processed  302 models on 5 predictors in 17.616310119628906 seconds.
Processed  301 models on 6 predictors in 17.06148409843445 seconds.
Processed  300 models on 7 predictors in 19.8487708568573 seconds.
Processed  299 models on 8 predictors in 30.17530918121338 seconds.
Processed  298 models on 9 predictors in 17.55156707763672 seconds.
Processed  297 models on 10 predictors in 16.646650075912476 seconds.
Processed  296 models on 11 predictors in 16.71415686607361 seconds.
Processed  295 models on 12 predictors in 16.437955856323242 seconds.
Processed  294 models on 13 predictors in 16.295653820037842 seconds.
Processed  293 models on 14 predictors in 16.39257502555847 seconds.
Processed  292 models on 15 predictors in 16.240917682647705 seconds.
Processed  291 models on 16 predictors in 16.22345805168152 seconds.
Processed  290 models on 17 predictors in 16.05762791633606 seconds.
Processed  289 models on 18 predictors in 16.36912775039673 seconds.
Processed  288 models on 19 predictors in 16.120107173919678 seconds.
Processed  287 models on 20 predictors in 16.062620878219604 seconds.
Processed  286 models on 21 predictors in 16.060329914093018 seconds.
Processed  285 models on 22 predictors in 16.031526803970337 seconds.
Processed  284 models on 23 predictors in 16.078983068466187 seconds.
Processed  283 models on 24 predictors in 15.974694967269897 seconds.
Processed  282 models on 25 predictors in 16.08683204650879 seconds.
Processed  281 models on 26 predictors in 15.962074041366577 seconds.
Processed  280 models on 27 predictors in 15.827784776687622 seconds.
Processed  279 models on 28 predictors in 15.896071910858154 seconds.
Processed  278 models on 29 predictors in 15.807521104812622 seconds.
Processed  277 models on 30 predictors in 16.23077392578125 seconds.
Processed  276 models on 31 predictors in 15.775439977645874 seconds.
Total elapsed time: 510.5404269695282 seconds.models_fwd
```

![](img/e2692858e93dee03687404e99d80669a.png)

```
sns.lineplot(data=models_fwd.iloc[:, :2])
plt.xlabel("Number of Variables")Text(0.5, 0, 'Number of Variables')
```

![](img/c9360e75fec8bd522ed29f3ca080b2dd.png)

我们看到，随着变量数量的增加，测试和训练数据集中的 RSS 减少。然而，RSS 下降的比率在 25 左右下降，这表明我们不会通过添加更多的变量来获得进一步的改善。所以我将把有 25 个变量的最佳模型作为我们的最终模型。

```
print(models_fwd.model[25].summary())OLS Regression Results                            
==============================================================================
Dep. Variable:              SalePrice   R-squared:                       0.893
Model:                            OLS   Adj. R-squared:                  0.891
Method:                 Least Squares   F-statistic:                     319.3
Date:                Sat, 04 Dec 2021   Prob (F-statistic):               0.00
Time:                        12:50:16   Log-Likelihood:                -11286.
No. Observations:                 978   AIC:                         2.262e+04
Df Residuals:                     952   BIC:                         2.275e+04
Df Model:                          25                                         
Covariance Type:            nonrobust                                         
========================================================================================
                           coef    std err          t      P>|t|      [0.025      0.975]
----------------------------------------------------------------------------------------
const                -9.915e+05   7.73e+04    -12.819      0.000   -1.14e+06    -8.4e+05
OverallQual           8989.6400   1066.511      8.429      0.000    6896.657    1.11e+04
GrLivArea               66.2250      2.761     23.989      0.000      60.807      71.643
BsmtFinSF1              18.5135      2.266      8.171      0.000      14.067      22.960
KitchenQual_Ex        2.204e+04   4594.511      4.798      0.000     1.3e+04    3.11e+04
RoofMatl_ClyTile     -6.581e+05   3.15e+04    -20.907      0.000    -7.2e+05   -5.96e+05
TotalBsmtSF             21.9704      2.664      8.248      0.000      16.743      27.198
YearBuilt              473.4111     39.665     11.935      0.000     395.571     551.251
BsmtQual_Ex            2.35e+04   3977.925      5.906      0.000    1.57e+04    3.13e+04
OverallCond           5476.8244    845.337      6.479      0.000    3817.885    7135.764
Neighborhood_NoRidge  3.756e+04   5410.253      6.942      0.000    2.69e+04    4.82e+04
RoofMatl_WdShngl      6.368e+04   1.54e+04      4.141      0.000    3.35e+04    9.39e+04
SaleType_New          2.024e+04   4407.987      4.591      0.000    1.16e+04    2.89e+04
Neighborhood_NridgHt  2.585e+04   4647.546      5.562      0.000    1.67e+04     3.5e+04
Neighborhood_StoneBr  4.311e+04   6435.717      6.698      0.000    3.05e+04    5.57e+04
Neighborhood_Crawfor  2.494e+04   4506.451      5.535      0.000    1.61e+04    3.38e+04
Condition2_PosN      -2.385e+05   1.89e+04    -12.588      0.000   -2.76e+05   -2.01e+05
BldgType_1Fam         2.509e+04   2316.256     10.830      0.000    2.05e+04    2.96e+04
ExterQual_Ex          2.988e+04   5899.804      5.064      0.000    1.83e+04    4.15e+04
BedroomAbvGr         -6080.9435   1328.144     -4.579      0.000   -8687.371   -3474.516
BsmtExposure_No      -1.355e+04   1799.059     -7.529      0.000   -1.71e+04      -1e+04
SaleCondition_Normal  7240.6328   2840.831      2.549      0.011    1665.619    1.28e+04
PoolArea                31.4177     24.948      1.259      0.208     -17.542      80.378
Neighborhood_Somerst  1.394e+04   3998.045      3.486      0.001    6091.750    2.18e+04
Functional_Typ        1.182e+04   3318.954      3.560      0.000    5303.730    1.83e+04
LowQualFinSF           -25.5738     18.256     -1.401      0.162     -61.401      10.253
==============================================================================
Omnibus:                      250.336   Durbin-Watson:                   2.062
Prob(Omnibus):                  0.000   Jarque-Bera (JB):             6090.210
Skew:                           0.573   Prob(JB):                         0.00
Kurtosis:                      15.171   Cond. No.                     2.67e+05
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 2.67e+05\. This might indicate that there are
strong multicollinearity or other numerical problems.
```

请注意，即使我们只使用了 306 个变量中的 25 个，仍然能够得到 90%的 R 值。线性模型最大的好处是我们可以解释它们，理解数据。例如，我们的模型选择房屋的整体质量作为强有力的预测指标。平均而言，房屋整体质量每提高一个单位，房屋销售价格就会上涨 8989 美元。另一方面，如果一个房子的屋顶有粘土瓦，它会大大降低房子的价值(65000 美元)。这两个发现都很直观。不是吗？

## 正则化模型选择—使用岭回归

岭回归通过限制回归系数的大小来缩小回归系数。在实践中，当我们预期数据中存在多重共线性时，最常使用这种方法。当多重共线性存在时，最小二乘估计是无偏的，但是它们的方差被夸大了，因此它们可能远离真实值。通过在回归估计中加入一定程度的偏差(惩罚),岭回归减少了标准误差。预计平均效果将是给出更可靠的估计。

像任何其他机器学习超参数一样，我们必须找到惩罚项的微调值。让我们试着找到它。

```
# number of alphas to try
n_alphas = 200

# rangs of alphas to try
alpha = np.linspace(1, 50, 50)

coefs = []

# getting coefficients in each iteration
for a in alpha:
    ridge = Ridge(alpha=a, fit_intercept=False, normalize=True)
    ridge.fit(X_train, y_train)
    coefs.append(ridge.coef_)

ax = plt.gca()

# plotting alphas and coef
ax.plot(alpha, coefs)
#ax.set_xscale("log")
ax.set_xlim(ax.get_xlim()[::-1])  # reverse axis
plt.xlabel("alpha")
plt.ylabel("coef")
plt.title("Ridge coefficients as a function of the regularization")
plt.axis("tight")
plt.show()
```

![](img/b18fa2b4047e1b487dcae22e090e3b00.png)

我们可以看到，最佳α值在 20 左右，此时所有系数都很稳定，我们仍然可以看到一些信号。

使用 sklearn 内置的交叉验证函数，有一种更简单的方法可以找到 alpha(惩罚项)。

```
cv_kfold = KFold(n_splits=20, random_state=1, shuffle=True)
alpha_range = np.linspace(1, 50, 50)
clf = RidgeCV(alphas = alpha_range, normalize=False, cv = cv_kfold).fit(X_train, y_train)clf.alpha_16.0
```

交叉验证模型选择 alpha = 16，这与我们在上面的图中看到的非常接近。我们来看看模型。

## 最能增加房子价值的特征

```
pd.DataFrame({"Feature":X_train.columns.tolist(),"Coefficients":clf.coef_})\
.sort_values(by = 'Coefficients', ascending=False)\
.head(20)\
.style.bar()
```

![](img/e1602bbc81f8dfc0cdb27a155eb24c06.png)

## 最能降低房屋价值的特征

```
pd.DataFrame({"Feature":X_train.columns.tolist(),"Coefficients":clf.coef_})\
.sort_values(by = 'Coefficients', ascending=True)\
.head(20)\
.style.bar()
```

![](img/1b54c89d8d7a0844238f609de3e26023.png)

## 结论:

我们在两个模型中看到一些相似的趋势，但是一些系数值有点不同。例如，我们通过子集选择方法选择的模型发现，有一个粘土屋顶会使房子的价值减少 65k，这比惩罚模型给出的结果(大约 15k 美元)要极端得多。直觉上，这是有意义的，因为在子集选择中，我们没有惩罚任何变量。在接下来的教程中，我会尝试解释为什么会出现这种情况，以及哪种模型更正确。