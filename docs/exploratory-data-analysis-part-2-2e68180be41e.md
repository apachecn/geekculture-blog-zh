# 探索性数据分析(第二部分)

> 原文：<https://medium.com/geekculture/exploratory-data-analysis-part-2-2e68180be41e?source=collection_archive---------12----------------------->

在探索性数据分析的第 1 部分中，我们讨论了如何通过数字来理解数据。在这一部分，我们将学习可视化数据，以获得它们之间的关系。

![](img/7d7f2df693ac4af3b1a9ffeaa36cd4dc.png)

这可以通过两种方式实现:

**第一种方式**:仅使用与绘图相关的列，查找列和绘图**热图**之间的**相关性**。

**第二种方式**:分别绘制**缺失**列、**连续**列、**离散**列、**分类**列之间的关系。

**使用第一种方式**:

在这里，我们必须计算数据集各列之间的相关性，使用这些相关值绘制热图，使用 Python 的 Seaborn 库函数进行可视化。

```
import seaborn as snscorrelation = data.corr()  
#It calculates the correlation value between each columnsns.heatmap(correlation, xticklabels= correlation.columns, yticklabels= correlation.columns, annot= False) 
```

这将绘制在热图上计算的相关值，xticklabels 和 yticklabels 是分别在 X 轴和 Y 轴上绘制的列的列表，annot 是 annotation 的缩写，如果设置为 True，则显示相关值

您也可以使用这个简单的代码绘制 PairPlot

```
sns.pairplot(data)
```

惊鸿一瞥，就在这里自己尝试一下，体验更好。

现在，在绘制热图和 Pairplot 之后，您可以观察相关的列，并且可以使用

```
sns.relplot(x= 'col_name', y= 'col_name', data= data, hue= 'col_name')
```

**使用第二种方式:**

这里我们将分别可视化**缺失**列、**连续**列、**离散**列和**分类**列之间的关系。

使用此从数据集的缺失值开始

```
data.isnull().sum()    #number of null values in each column
```

如果数据集包含大量的空值**和空值**，那么使用

```
feature_na = [features **for** features **in** data.columns **if** data[features].isnull().sum()>1]
#This makes list of columns which contains **null** values **for** feature **in** feature_na:
 data_temp = data.copy()    
#To not to change our orginal data data_temp[feature] = np.where(data_temp[feature].isnull(),1 ,0)
 #Here 1 indicates missing value and 0 indicates not missing value data_temp.groupby(feature)['target_col'].median().plot.bar()
 plt.title(feature)
 plt.show()
```

如果数据集有大量缺失值，请使用此代码，否则请使用

```
sns.countplot(x= 'Col_name', data= data) #other parameters can also be used which are not specified here
```

现在来看数据集的数字列

```
features_num = [feature **for** feature **in** dataset.columns **if** dataset[feature].dtypes != 'O']
#The columns which are **not** of **Object type** are Numerical column
```

有两种类型的数字列:

1.  离散列(包含特定的可计数值，如计算机数量、购买的杂货数量)
2.  连续列(包含特定范围内的任何测量值，如每日温度、班级学生的体重)

让我们来看看离散列的可视化

```
feature_dis =  [feature **for** feature **in** feature_num **if** data[feature].nunique()<25]
#This makes list of Columns containing **Discrete** columns
#This 25 value can be changed according to dataset**for** feature **in** feature_dis:
    data_temp=data.copy() data_temp.groupby(feature)['Target_col'].median().plot.bar()
    #This will group by each column containing Discrete value and Target_col plt.xlabel(feature)
    plt.ylabel('Target_col')
    plt.title(feature)
    plt.show()
```

这段代码将帮助您可视化离散列和 Target_col 之间的关系

对于连续柱的可视化

```
features_cont = [feature **for** feature in features_num **if** feature **not in** feature_dis]
#The columns which are not Discrete columns are Continuous columns
#This makes list of Continuous Columns**for** feature **in** continuous_feature:
    data_temp=data.copy()
    data_temp[feature].hist(bins=25) 
    #Continuous columns have continuous values, plot it on histogram plt.xlabel(feature)
    plt.ylabel("Count")
    plt.title(feature)
    plt.show()
```

这段代码将帮助你可视化直方图上连续列之间的关系

现在来看数据集的**分类列**

分类列是将数据分成性别、年龄组、教育水平、种姓、阶层等组的列。

对于分类列的可视化

```
feature_cat = [feature **for** feature **in** data.columns **if** data[feature].dtypes=='O']
#The Categorical columns are of **Object type****for** feature **in** feature_cat:
    data_temp=data.copy()
    data_temp.groupby(feature)['Target_col'].median().plot().bar()
    #This will group by all Categorical columns with Target_col plt.xlabel(feature)
    plt.ylabel('Target_col')
    plt.title(feature)
    plt.show()
```

这段代码将帮助您可视化分类列和 Target_col 之间的关系。

到目前为止，我们已经介绍了如何通过各种数据的可视化来理解数据集，如离散数据、连续数据和分类数据。在第 3 部分中，我提出了可以由一行代码执行的 EDA 技术，整个可视化就完成了。第 3 部分即将推出。