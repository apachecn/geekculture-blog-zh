# 从熊猫的数据框架生成自定义地图

> 原文：<https://medium.com/geekculture/generating-a-custom-map-from-pandas-dataframe-48638926464c?source=collection_archive---------41----------------------->

你准备好为你的项目创建一些自定义的开源地图了吗？嗯，我也没有，但是我们将从一小步开始。让我们开始吧。

![](img/25f66b45ebda4a96b359be394227c0ca.png)

Photo by [Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

让我们导入我们将在这个项目中使用的重要 python 库。这些图书馆是“[叶子](http://python-visualization.github.io/folium/)”、“[熊猫](https://pandas.pydata.org/)”和“[地质公园](https://geopy.readthedocs.io/en/stable/)”。

这是我们的第一段代码。我们将使用 ArcGIS 的 API，并将它初始化为一个函数，并将其分配给 nom 作为命名者。

```
import folium
import pandas
from geopy import ArcGIS
nom = ArcGIS()
```

我们在这个项目中的主要目标是获得用户输入，因为我们将从用户那里收集适当的地址，然后使用 pandas 库，我们将生成一个数据帧，并将收集的数据连接为完整的地址。在这一阶段，geopy 将获得我们的帮助，它将使用地理编码功能从地址中提取坐标。

为了收集正确的地址，我们使用以下代码片段:

```
print(“Please enter your address accordingly!”)
address = input(“Address: “)
city = input(“City: “)
state = input(“State: “)
zip = input(“Zip: “)
```

我们将收集，“地址”为“密歇根大道北 25 号”，“城市”为芝加哥，“州”为 IL，最后，“邮政编码”为 60602。

```
df = pandas.DataFrame([[address,city,state,zip]],columns=[“address”,”city”,”state”,”zip”])df[“Full Address”] = df[“address”]+”,”+df[“city”]+”,”+df[“state”]+ “,”+ str(df[“zip”])
```

收集完所需的地址后，我们将使用 pandas 库并创建一个数据框，在该数据框中，数据将保存为列表。为了使用 ***地理编码*** 我们需要将收集到的数据形成一个整体。因此，我们连接这些列，并将在数据帧中创建另一列。

```
df[“Coordinates”] = df[“Full Address”].apply(nom.geocode)
```

然后，在 ***nom.geocode*** 函数的帮助下，我们将提取经纬度并保存在 DataFrame 的另一列中。现在，数据帧中的“坐标”列包含纬度和经度信息。

```
latitude = df[“Latitude”] = df[“Coordinates”].apply(lambda la : la.latitude if la != None else None)longitude = df[“Longitude”] = df[“Coordinates”].apply(lambda lo : lo.longitude if lo != None else None)
```

在上面的代码片段中，我们获得了精确的坐标，并将它们用作 leav 中的输入。Map()函数，创建一个地图。让我们看看下面的片段。在这段代码中，我们创建了一个名为 map 的变量，并将其赋给一个名为 follow 的函数。Map()，它有一个参数“location ”,该参数接受一个列表，由纬度和经度组成。

```
map = folium.Map(location=[latitude,longitude])
map.save(“map1.html”)
```

总之，我们将使用 save()函数用 follow python 库创建一个名为“map1.html”的新地图。

在这里你可以访问整个代码:[https://github.com/bunyaminsari/map](https://github.com/bunyaminsari/map)

不要忘记检查这个链接；[https://benedtech.medium.com/membership](https://benedtech.medium.com/membership)