# 从谷歌计算引擎使用熊猫数据框架创建大查询表

> 原文：<https://medium.com/geekculture/create-bigquery-table-using-pandas-dataframe-from-google-compute-engine-6f06669a5dd1?source=collection_archive---------3----------------------->

![](img/3887b3fd15393ecf9ec1df9983c95cbf.png)

Photo by [Tobias Fischer](https://unsplash.com/@tofi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果您通过 VM 实例在 Google Compute Engine (GCE)中工作，您可以从 pandas dataframe 创建 BigQuery 表。dataframe 的每一列将在 BigQuery 表中创建一个单独的字段。

在下面，我描述了你如何做到这一点。

**步骤 1:更改 GCP 虚拟机实例上的 API 访问范围**

将 VM 实例中的访问范围从`Allow default access`更改为`Allow full access to all Cloud APIs`,以授予 VM 实例在 BigQuery API 上写入的权限。

![](img/0a1e411077884b9a7d8753079441803a.png)

**步骤 2:安装依赖关系**

我们将使用 [pandas_gbq](https://pandas-gbq.readthedocs.io/en/latest/index.html) 模块从我们的 pandas 数据帧创建 bigquery 表。因此，首先，在您的虚拟环境中安装该模块。

```
# using pip
$ pip install pandas-gbq -U# using conda
$ conda install pandas-gbq --channel conda-forge
```

**第三步:在 BigQuery 主页中创建数据集**

转到您的 BigQuery 控制台，您可以使用以下 URL 转到您的 BigQuery 控制台页面。

```
[https://console.cloud.google.com/bigquery?project=](https://console.cloud.google.com/bigquery?project=)your-project-name# replace your-project-name with the project name that you're 
# working on GCP. 
```

如果没有，在 bigquery 页面上创建一个新的数据集。假设我们创建了一个名为`finance-dept`的数据集。现在，我们想为我们的员工信息创建一个表，假设表名为`emp_info`。

**步骤 4:使用 Pandas Dataframe 创建 BigQuery 表的代码**

创建 bigquery 表的代码:

在上面的代码中，

在我们的例子中，`dataset_tablename`应该是`finance-dept.emp_info`。`gcp_project_name`是你的 GCP `Project ID`。这将根据每个列的数据类型为您的表自动创建一个模式。您不需要手动创建它。

有 3 种不同的方法可用于`if_exists`参数。请在其文档页面上查看每种方法的详细信息。

 [## 熊猫。DataFrame.to_gbq - pandas 1.3.4 文档

### 将 DataFrame 写入 Google BigQuery 表。这个函数需要 pandas-gbq 包。请参阅如何…

pandas.pydata.org](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_gbq.html) 

现在，通过访问以下 URL，您可以在左侧面板中找到由上述代码创建的表:

```
[https://console.cloud.google.com/bigquery?project=](https://console.cloud.google.com/bigquery?project=gaishoku-bi-trend-project)<PROJECT-ID>
```

用您的 GCP 项目 ID 替换`<PROJECT_ID>`。

谢谢你可以在 LinkedIn 上联系我。

[](https://www.linkedin.com/in/sksoumik/) [## Sadman Kabir Soumik -人工智能工程师- Venturas Ltd | LinkedIn

### 查看 Sadman Kabir Soumik 在全球最大的职业社区 LinkedIn 上的个人资料。萨德曼·卡比尔有两份工作…

www.linkedin.com](https://www.linkedin.com/in/sksoumik/)