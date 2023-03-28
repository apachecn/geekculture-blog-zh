# 如何正确地在 Apache Spark 上执行 REST API 调用

> 原文：<https://medium.com/geekculture/how-to-execute-a-rest-api-call-on-apache-spark-the-right-way-in-python-4367f2740e78?source=collection_archive---------3----------------------->

![](img/518781cce0bd203b594e0b8fd3d2bff1.png)

Photo by [Jez Timms](https://unsplash.com/@jeztimms?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

本文使用 Python 作为例子。对于那些正在寻找 Scala 解决方案的人来说，这些理论和方法是完全适用的，请查看我的 Github repo 中的 Scala 源代码【https://github.com/jamesshocking/Spark-REST-API-UDF-Scala】[](https://github.com/jamesshocking/Spark-REST-API-UDF-Scala)**。**

*注:2022 年 10 月，REST API 终点([https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?演示代码中使用的 format=json](https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json) )在调用者(Spark 代码)没有识别出是什么浏览器类型(即 Chrome、Edge)时不再接受请求。当向远程服务发出请求导致没有数据返回时，会引发异常。当执行演示代码时，请为您的实验和测试尝试不同的端点。*

*Apache Spark 是一项了不起的发明，可以解决许多问题。它的灵活性和适应性给了我们巨大的力量，但也给了我们犯大错误的机会。其中一个错误是在驱动程序上执行代码，您认为它会以分布式方式在工作程序上运行。一个这样的例子是当您在 Dataframe 的上下文之外执行 Python 代码时。*

*例如，当您执行类似于以下代码时:*

```
*s = "Python syntax highlighting"
print s*
```

*Apache Spark 将在驱动程序上执行代码，而不是在工作程序上。对于这样一个简单的命令来说这不是问题，但是当您需要通过 REST API 服务下载大量数据时会发生什么呢？*

```
*import requests
import jsonres = Nonetry:
  res = requests.get(url, data=body, headers=headers)

except Exception as e:
  print(e)if res != None and res.status_code == 200:
 print(json.loads(res.text))*
```

*如果我们执行上面的代码，它将在驱动程序上执行。如果我要创建一个包含多个 API 请求的循环，那么就没有并行性，没有伸缩性，对驱动程序有很大的依赖性。这种方法扼杀了 Apache Spark，使它比单线程 Python 程序好不了多少。为了利用 Apache Spark 的伸缩性和分布性，必须寻找一种替代解决方案。*

*解决方案是使用与 withColumn 语句耦合的 UDF。这个例子演示了如何创建一个数据帧，每一行代表一个对 REST 服务的请求。使用 UDF(用户定义的函数)封装 HTTP 请求，返回一个表示 REST API 响应的结构化列，然后可以使用 explode 和其他内置的 DataFrame 函数(或折叠，参见[https://github.com/jamesshocking/collapse-spark-dataframe](https://github.com/jamesshocking/collapse-spark-dataframe))对其进行分割。*

# *解决方案*

*为了简洁起见，我假设已经创建了一个 SparkSession，并将其分配给一个名为 spark 的变量。此外，对于这个例子，我将使用 Python 请求 HTTP 库。*

*该解决方案假设您需要使用 REST API 中的数据，您将多次调用该 API 来获取您需要的数据。为了利用 Apache Spark 提供的并行性，每个 REST API 调用都将由一个 UDF 封装，该封装绑定到一个数据帧。数据帧中的每一行都代表对 REST API 服务的一次调用。一旦在数据帧上执行了一个动作，每个单独的 REST API 调用的结果将作为结构化数据类型附加到每一行。*

*为了演示这个机制，我将使用一个免费的美国政府 REST API 服务，该服务返回美国汽车的品牌和型号[https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json](https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json) 。*

# *从申报进口开始:*

```
*import requests
import json
from pyspark.sql.functions import udf, col, explode
from pyspark.sql.types import StructType, StructField, IntegerType, StringType, ArrayType
from pyspark.sql import Row*
```

# *现在声明一个将执行 REST API 调用的函数*

*使用请求库执行 HTTP get 或 post。这个函数没有什么特别的，除了 REST 服务响应将作为一个 JSON 对象传递回来。*

```
*def executeRestApi(verb, url, headers, body):
  #
  headers = {
      'content-type': "application/json"
  } res = None
  # Make API request, get response object back, create dataframe from above schema.
  try:
    if verb == "get":
      res = requests.get(url, data=body, headers=headers)
    else:
      res = requests.post(url, data=body, headers=headers)
  except Exception as e:
    return e if res != None and res.status_code == 200:
    return json.loads(res.text) return None*
```

# *定义响应模式和 UDF*

*这是 Apache Spark 中我非常喜欢的部分之一。我可以从 REST API 调用返回的 JSON 中挑选我想要的值。我所要做的就是在声明模式时，我只需要确定我想要 JSON 的哪些部分。*

```
*schema = StructType([
  StructField("Count", IntegerType(), True),
  StructField("Message", StringType(), True),
  StructField("SearchCriteria", StringType(), True),
  StructField("Results", ArrayType(
    StructType([
      StructField("Make_ID", IntegerType()),
      StructField("Make_Name", StringType())
    ])
  ))
])*
```

*接下来，我声明 UDF，确保将返回类型设置为我声明的模式。这将确保用于执行 UDF 的新列最终包含结构化对象形式的数据，而不是普通的 JSON 格式的文本。该操作类似于使用 from_json 函数，该函数将模式作为其第二个参数。*

```
*udf_executeRestApi = udf(executeRestApi, schema)*
```

# *创建请求数据帧并执行*

*最后一部分是创建一个数据帧，其中每一行代表一个 REST API 调用。Dataframe 中的列数由您决定，但您至少需要一列，它将存放执行 REST API 调用所需的 URL 和/或参数。我将使用四个来反映 REST API 调用函数需要的单个参数的数量。*

*使用美国政府的免费访问车辆 make REST 服务，我们将创建如下数据框架:*

```
*from pyspark.sql import Rowheaders = {
    'content-type': "application/json"
}body = json.dumps({
})RestApiRequestRow = Row("verb", "url", "headers", "body")
request_df = spark.createDataFrame([
            RestApiRequestRow("get", "https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json", headers, body)
          ])*
```

*Row 类用于定义 Dataframe 的列，并使用 spark 对象的 createDataFrame 方法，为我们要进行的每个 API 调用声明一个 RestApiRequestRow 实例。*

*如果一切顺利，数据帧看起来会像这样:*

*动词、url、标题、正文【https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?】get、[T2format=json](https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json) ，{ ' content-type ':" application/JSON " }，{}*

*最后，我们可以在数据帧上使用 withColumn 来执行 UDF 和 REST API。*

```
*result_df = request_df \
             .withColumn("result", udf_executeRestApi(col("verb"), col("url"), col("headers"), col("body")))*
```

*由于 Spark 比较懒惰，一旦对数据帧执行了 count()或 show()之类的操作，UDF 就会执行。Spark 将在返回结果之前，将 API 调用分配给所有的工人，例如:*

*动词，网址，标题，正文，结果
获取，[https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json](https://vpic.nhtsa.dot.gov/api/vehicles/getallmakes?format=json) {'content-type ':"应用程序/json"}，{}，[9773，响应 r…]*

*REST 服务返回许多属性，我们只对标识为 Results 的属性感兴趣(即 result。结果)。如果我们使用我的 collapse_columns 函数([https://github.com/jamesshocking/collapse-spark-dataframe](https://github.com/jamesshocking/collapse-spark-dataframe)):*

```
*df = result_df.select(explode(col("result.Results")).alias("results"))
df.select(collapse_columns(df.schema)).show()*
```

*你会看到:*

```
*results_Make_ID, results_Make_Name 
440, ASTON MARTIN 
441, TESLA 
442, JAGUAR 
443, MASERATI 
444, LAND ROVER 
445, ROLLS ROYCE
...*
```

*本文的源代码可以在我位于 https://github.com/jamesshocking/Spark-REST-API-UDF[的 Github 上找到。](https://github.com/jamesshocking/Spark-REST-API-UDF)*