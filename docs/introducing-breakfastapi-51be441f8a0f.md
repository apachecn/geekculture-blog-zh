# 简介:早餐 Api🍕

> 原文：<https://medium.com/geekculture/introducing-breakfastapi-51be441f8a0f?source=collection_archive---------19----------------------->

web 上最美味的开源 API。

![](img/f3fddbdb28e771aa4c28d523c9be3133.png)

Image by the author.

早餐 Api 立志成为 web 上最美味的开源 API。只需发送一个请求，你就会收到最令人垂涎的菜肴配方，包括预计烹饪时间、所有必要的配料和说明。

# **动机**

创建 BreakFastApi 是为了解决每个人在生活中的某个时刻都面临的一个挑战性问题。**晚饭做什么？**我喜欢烹饪，但这个问题在疫情变得更加普遍，人们被迫呆在家里，想出新的令人兴奋的饭菜来做。

## 问题:

人类只能记住有限数量的食物食谱。然而，每个人都将受益于更加多样化和丰富的饮食。

## 解决方案:

早餐 Api 通过记忆超过 12.000 个食谱并使它们随意可用来解决这个问题。美味的饭菜现在离你只有一步之遥。

# 它是如何工作的？

api 有一个超过 12000 种食谱的数据库，包括估计的烹饪时间、配料和说明。api 一收到请求，就会从数据库中查询一个随机条目，并以 JSON 格式返回食谱。

顾名思义，它是使用 FastAPI 构建的，您可以在这里查看源代码:

[](https://github.com/MariiaSizova/breakfastapi) [## GitHub—MariiaSizova/breakfast api:web 上最美味的 API。🍣 🍔 🍕

### 网络上最美味的 API。只需发送一个请求，你就会收到最令人垂涎欲滴的菜肴配方…

github.com](https://github.com/MariiaSizova/breakfastapi) 

它在 [**Deta**](https://www.deta.sh/) 举办。不太知名，但对 Python 和节点开发者来说是一个非常棒的免费托管平台。绝对值得一探究竟！

# 有什么好尝的菜谱？

## [早餐 API.fun](https://breakfastapi.fun/)

# 机器人呢？

如果你是计算型的，你可以通过基本的 python 代码与 API 接口，并在你的应用程序中实现这些方法。

**样品申请:**

```
import requests
r = requests.get(url='https://breakfastapi.fun/')
data = r.json()
print(data)
```

**样本响应:**

```
{
    'ID':11574,
    'Recipe Name': 'Devils Steak Sauce Recipe',
    'Cook Time (minutes)': 15,
    'Ingredients': ['brown sugar',
                    'tomato sauce',
                    'raspberry',
                    'worcestershire sauce',
                    'hot pepper',
                    'black pepper',
                    'vinegar'
                    ],
    'Directions': 'In a saucepan over high heat, 
                   blend raspberry jam, brown sugar, 
                   Worcestershire sauce, tomato sauce, 
                   malt vinegar, hot pepper sauce, salt, 
                   and pepper. Bring to a boil over high heat, 
                   reduce heat to low, and simmer 10 minutes,
                   or until thickened.'
}
```

## **代码分解**

**main.py:** 这里我们初始化主 FastAPI 和数据库对象，并注册索引路径。索引路由调用 jsonify_data()方法，将数据库条目转换为 JSON 格式。

```
import uvicorn
from fastapi import FastAPI
from db.database import Database
from helpers.data_formating import jsonify_data app = FastAPI()
db = Database() @app.get("/", status_code=200)
def get_recipes() -> list:
    return jsonify_data(db)if __name__ == "__main__":
    uvicorn.run(app, host="127.0.0.1", port=8000)
```

**database.py:** 定义了主要的 sqlite3 数据库类。它有一个额外的方法 get_data，该方法从数据集中返回一个随机行。然后，这个随机行通过 jsonify_data()函数传递，并在端点收到 GET 请求时返回给用户。

```
import sqlite3 class Database:
    def __init__(self):
        self.connection = sqlite3.connect("db/recipes.db",   check_same_thread=False)
       self.cursor = self.connection.cursor() def get_data(self):
        self.cursor.execute("SELECT * FROM items ORDER BY RANDOM() LIMIT 1")
    return self.cursor.fetchone()
```

## 数据源

API 使用了修改和扩充的版本，你可以在 Kaggle 上找到这个优秀的数据集。

[](https://www.kaggle.com/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv) [## Food.com 食谱和互动

### 从 Food.com(GeniusKitchen)在线食谱聚合器抓取数据

www.kaggle.com](https://www.kaggle.com/shuyangli94/food-com-recipes-and-user-interactions?select=RAW_recipes.csv) 

感谢您的阅读！希望这个 API 对你❤️有所帮助