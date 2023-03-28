# 带有 Python 和 MongoDB 图表的金融公司数据仪表板

> 原文：<https://medium.com/geekculture/financial-companies-data-dashboard-with-python-and-mongodb-charts-b23a48fcd7ed?source=collection_archive---------5----------------------->

![](img/f327fdfd26097f6d6e8eada4cca29583.png)

Photo by [Tech Daily](https://unsplash.com/@techdailyca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 作为一名股票投资者，我一直在寻找新的方法来改善我的交易策略。最重要的事情之一是对我投资的公司的财务状况有一个很好的了解。

我一直在寻找一种方法来自动化获取数据的过程，并在一个整洁的仪表板中可视化它们。

*那么，如何获得我的股票的关键财务指标，并在仪表板中显示出来呢？*

## 介绍

在这个项目中，我将向你展示如何获得我投资的公司的财务状况，并使用 **Python、雅虎财经 python API 和 MongoDB 图表在仪表板上可视化它们。**

MongoDB Charts 最酷的特性之一是很容易在网页上共享，只需点击几下，了解一点 html 和 CSS 知识(稍后我将展示如何创建一个简单的 HTML 页面并嵌入 Mongo Charts 仪表板)。

**这个项目可以定义为一个基础数据工程项目。该脚本由典型的数据工程管道或 ETL(提取、转换、加载)管道构成。**你可以在我的 [GitHub Repo](https://github.com/marcello-calabrese/financial_data_companies_mongo) 里找到该项目的代码。

这个项目的最终结果是一个 html 页面，显示仪表板并从 MongoDB 图表中获取数据。仪表板每天都会更新我投资组合中公司的最新财务状况。

![](img/04dd0ba9d4a977f53d03909e70cb096c.png)

Web page dashboard: image by author

## **获取我交易的公司的财务信息**

幸运的是，有一个名为 yfinance 的库可以帮助我们获得财务数据。

通过简单的代码语法，我们可以获得信息，如最新的股价，市值，测试，52 周的范围，收益日期，股息收益率，市盈率等。

我决定获取我的投资组合中所有公司的财务信息，并将它们存储在云中的数据库中，而 *MongoDB* 是这个项目的完美选择，因为它免费且易于使用(免费层允许您存储 512 MB 的数据)。

目前我投资组合中的股票有:

**-微软**

**-麦当劳**

**- IBM**

**-约翰逊&约翰逊**

**-锡安银行**

我决定将这些公司的财务数据存储在数据库中:

**-符号或股票**

**-公司名称**

**-公司描述**

**- PE 比率(市盈率)，如果你不知道什么是 PE 比率，可以在这里**[](https://www.investopedia.com/terms/p/pe.asp)**了解更多**

****-公司分红****

****-现金流****

***现在让我们运行代码，看看如何获取我的投资组合中公司的财务信息，并将它们存储在 MongoDB 数据库中。***

## ****创建一个 MongoDB 帐户****

**我不会详细介绍如何创建一个 MongoDB 帐户，下面的链接将帮助您完成这个过程:**

*   **创建 MongoDB 免费账号:[链接](https://www.mongodb.com/cloud/atlas)**
*   **创建一个集群:[链接](https://docs.atlas.mongodb.com/tutorial/create-new-cluster/)**
*   **创建一个数据库和一个集合:[链接](https://docs.atlas.mongodb.com/tutorial/create-new-database/)**

## ****导入库****

**为了容易地复制我的项目，我在我的 GitHub 存储库中提供了一个 *requirements.txt* 文件。您可以使用以下命令安装所有库:**

```
pip install -r requirements.txt
```

**在新创建的环境中，创建一个新文件夹并创建一个新文件“”。py“”。**我调用了(你可以在我的回购中看到)run_mongo.py** 。在此文件中，您需要导入以下库:**

```
# import main libraries

import pandas as pd # data manipulation
import numpy as np # data manipulation
import yfinance as yf # get financials of the companies
import pymongo # connect to MongoDB
from pymongo import MongoClient # connect to MongoDB
import json # convert data to json
```

## ****ETL 管道:提取、转换、加载****

**现在在*里。py* 文件，我们将创建 **ETL 管道来从 yahoo finance 中提取数据，转换数据并将数据加载到 MongoDB 中。****

## ****从雅虎财经提取公司财务数据****

**提取函数 *extract_ticker()* 获取我的投资组合中的公司股票，创建一个字典来存储每个公司的上述数据，并将它们存储在 pandas 数据框架中。该函数返回熊猫数据帧。**

```
def extract_ticker():
    # extract the ticker data

    msft = yf.Ticker('MSFT')
    zion = yf.Ticker('ZION')
    ibm = yf.Ticker('IBM')
    jnj = yf.Ticker('JNJ')
    mcd = yf.Ticker('MCD')
    # create a dictionary to store the data
    dct2 = {'Company_name': [msft.info['longName'], zion.info['longName'], ibm.info['longName'], jnj.info['longName'], mcd.info['longName']],
                    'Company_ticker': [msft.info['symbol'], zion.info['symbol'], ibm.info['symbol'], jnj.info['symbol'], mcd.info['symbol']],
                    'Closed_price': [msft.info['previousClose'], zion.info['previousClose'], ibm.info['previousClose'], jnj.info['previousClose'], mcd.info['previousClose']],
                    'Company_info': [msft.info['longBusinessSummary'], zion.info['longBusinessSummary'], ibm.info['longBusinessSummary'], jnj.info['longBusinessSummary'], mcd.info['longBusinessSummary']],
                    'Company_PE': [msft.info['trailingPE'], zion.info['trailingPE'], ibm.info['trailingPE'], jnj.info['trailingPE'], mcd.info['trailingPE']],
                    'Company_cash_flow': [msft.info['operatingCashflow'], zion.info['operatingCashflow'], ibm.info['operatingCashflow'], jnj.info['operatingCashflow'], mcd.info['freeCashflow']],
                    'Company_dividend': [msft.info['dividendRate'], zion.info['dividendRate'], ibm.info['dividendRate'], jnj.info['dividendRate'], mcd.info['dividendRate']]}

    # create a dataframe to store the data
    df = pd.DataFrame(dct2)
    # return the dataframe
    return df
```

## ****转换数据****

**转换函数 *transform_data()* 应用一个小的数据清理，将值舍入到 2 个小数位，并将数据转换为稍后将数据加载到 MongoDB 所需的 json 格式。**

```
def transform_data():
    # create a dataframe to store the data
    df_transformed = extract_ticker() # call the extract function
    # round the values of the dataset to 2 decimal places
    df_transformed = df_transformed.round(2)

    # convert the dataframe into a json file
    json_file = df_transformed.to_json('Data/yahoo_data.json' ,indent=4, orient='records')
    return json_file
```

## ****将数据加载到 MongoDB****

**加载函数 *load_mongo()* 将数据加载到 MongoDB 中。该函数将连接字符串、数据库名称、集合名称和 json 文件作为参数。该函数返回加载到 MongoDB 中的数据。**

```
def load_mongo():
    # load the json file into mongodb
    # create a client object
    client = MongoClient('mongodb+srv://USER:PASSWORD@cluster0.ragcpnf.mongodb.net/?retryWrites=true&w=majority') # insert your connection string and your personal user and password of your MongoDB account
    # get a database named "stockdb"
    db1 = client.stockdb
    # get a collection named "yahoo_data_test_json"
    collection_main = db1.stock_data
    # load a json file into mongodb
    with open('Data/yahoo_data.json') as file:
        file_data = json.load(file)
    # insert_many is used else insert_one is used
    if isinstance(file_data, list):
        # empty the collection
        collection_main.delete_many({})
        # insert the data into the collection
        collection_main.insert_many(file_data)
    else:
        collection_main.insert_one(file_data)
    return True
```

## ****运行 ETL 管道****

**代码的最后一部分是运行 ETL 管道。下面的代码调用这些函数并打印结果。**

**我使用标准的 *def main()* 函数作为摄取 3 个 ETL 函数的主要部分，并使用*if _ _ name _ _ = _ _ main _ _:*来运行代码。**

```
def main():
    # call the extract function
    #extract_ticker()
    # call the transform function
    transform_data()
    # call the load function
    load_mongo()
    return True

### Run Layer ###

if __name__ == '__main__':
    main()
    print('Data has been extracted, transformed, saved in a json file and loaded into mongodb')
```

## ****MongoDB 数据库，我投资组合中的公司数据存储在一个集合中****

**执行代码后，我们可以在 MongoDB 数据库中看到数据。我创建了一个名为 *stockdb* 的数据库和一个名为 *stock_data* 的集合。数据存储在集合中。提醒一下，在 MongoDB 中，集合类似于关系数据库中的表，文档类似于关系数据库中的行。**

****请看下面 MongoDB 中的数据截图。****

**![](img/f06c5402eab8045038472cda2715252b.png)**

**MongoDB Database with the data of the companies in my portfolio stored in a collection**

**你可以看到这个脚本是如何捕捉我的投资组合中的公司的关键指标的。对于以前从未使用过 MongoDB 的人来说，这些数据看起来类似于字典列表，与我们在关系数据库中看到的数据完全不同。**

***太好了！我们在 MongoDB 里有数据。现在让我们让这些数据看起来有意义。***

****现在我们开始项目的第二个有趣部分，用 MongoDB 图表实现 MongoDB 内部的数据可视化**😎！**

## ****MongoDB 图表****

**现在我们开始这个项目的第二个有趣的部分。**

**我将向您展示用 MongoDB 图表创建一个仪表板有多简单，只需点击几下，并使用拖放工具在轴上绘制数据。每次执行 ETL 管道时，仪表板都会自动更新。仪表板将显示 6 个图表，其中包含我投资组合中公司的关键指标和信息。**

## ****创建仪表板****

**我们需要创建一个新的仪表板。为此，我们需要单击“创建仪表板”按钮。**请看下面的短视频演示。****

**可视化由两个表组成，分别是公司名称和描述以及公司名称和股票代码，其余的图表是简单的条形图，显示我的投资组合中公司的关键指标。**

****见下面视频演示的第二部分绘制图表。****

## ****导出仪表盘****

**创建仪表板后，我们可以使用 iframe 工具将仪表板导出并嵌入到网页中。**

****看下面的视频演示。****

## ****嵌入仪表板的基本 html 页面****

**最后一步是创建一个基本的 html 页面来嵌入仪表板。你可以在【github 资源库】(https://github . com/marcello-cala brese/financial _ data _ companies _ mongo/tree/main/html _ dashboard)和下面找到代码。我用 [Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/introduction/) CSS 框架创建了一个简单的页面。**

****参见下面的 html 代码:****

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- CSS only -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
    <title>Portfolio' Shares Financial Data Dashboard</title>
</head>
<body>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-12">
                <h1>Portfolio's Shares Financial Data Dashboard</h1>
            </div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <h2>Embedded MongoDB Dashboard directly from MongoDB Charts just with the iframe tags</h2>
            </div>
        </div>
        <div class="row">

                <!-- Add the iframe code generated from MongoDB Charts-->
                <iframe style="background: #F1F5F4;border: none;border-radius: 2px;box-shadow: 0 2px 10px 0 rgba(70, 76, 79, .2); height: 700px;"  src="https://charts.mongodb.com/charts-project-0-mohor/embed/dashboards?id=6379f795-14ab-4c8d-8ceb-051750764578&theme=light&autoRefresh=true&maxDataAge=3600&showTitleAndDesc=false&scalingWidth=fixed&scalingHeight=fixed"></iframe>
        </div>
    </div>
</body>
</html>
```

****页面短视频演示如下。**不要忘记用 MongoDB 图表生成的代码更改 iframe 代码，我必须手动将 iframe 中的高度像素添加到 700px，以确保仪表板在整个 html 页面中展开:**

## ****结论****

**在本文中，我向您展示了使用 MongoDB 图表创建仪表板是多么容易。我使用 ETL 管道从 web 中提取数据，转换数据并将其加载到 MongoDB 中。然后我用 MongoDB 图表创建了一个仪表板，并用 iframe 工具导出了仪表板，将其嵌入到一个基本的 html 页面中。**

****我希望你喜欢这篇文章，并学到一些新东西。如果你有任何问题，请在下面的评论中告诉我。感谢阅读！****

# **作者:**

**我写关于数据科学、python 编码项目和数据驱动营销的文章。我还为数据新手或数据入门者提供数据和业务指导。**

**你可以在[媒体](/@Marcello_Dichiera)和[推特](https://twitter.com/ItalyMarcello)上关注我，或者访问我的[网站](https://marcello-personal-website.netlify.app/)和 [Github 页面](https://github.com/marcello-calabrese)。**