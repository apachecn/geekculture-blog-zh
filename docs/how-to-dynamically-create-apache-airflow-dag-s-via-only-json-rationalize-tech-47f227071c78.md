# 如何仅通过 JSON 动态创建 Apache Airflow DAG 合理化技术

> 原文：<https://medium.com/geekculture/how-to-dynamically-create-apache-airflow-dag-s-via-only-json-rationalize-tech-47f227071c78?source=collection_archive---------10----------------------->

![](img/b98b48e2140597eee9b8c73690f6bf9b.png)

我们正在建立一个平台，用户可以来定义一个类似于 [IFTTT](https://ifttt.com/explore/welcome_to_ifttt?utm_source=IFTTT&utm_medium=Tout&utm_campaign=Signedout_Audience) 的工作流，我们在内部将这些任务分配给各种工具， [Apache airflow](https://airflow.apache.org/) 就是其中之一。因为我们事先不知道工作流的定义是什么，所以我们需要在运行中创建气流 DAG。让我们看看我们是如何做到这一点的。

# 正在创建 DAG 对象

由于 Airflow 考虑任何 python 文件(。py 扩展名)作为潜在的 DAG，并查找 DAG 的定义。我们所要做的就是动态创建 DAGs 对象。这可以通过下面的代码轻松实现。

```
from airflow import DAG 
default_args = { 
   "owner": "airflow",
   "start_date": datetime(2018, 1, 1),
   "email": ["someEmail@gmail.com"],
   "email_on_failure": False, 
} with DAG( "DAG Name", schedule_interval=None,   default_args=default_args ) as dag:
```

# 动态定义运算符

一旦我们定义了 **DAG** 对象，第二个任务就是动态定义操作符，并将它们添加到创建的 DAG 中。我们现在需要的是所有相关操作符及其参数的表示。下面是可以用来做这件事的 JSON。

```
[ { 
    "name": "Start",
    "_type": "airflow.operators.dummy_operator.DummyOperator",           "parameters": {},
 }, { 
   "name": "HTTPRequest",
   "_type": "airflow.operators.http_operator.SimpleHttpOperator",   "parameters": { 
  "http_conn_id": "dummyapi",
  "method": "GET", 
  "endpoint": "/api/users/2",
  "headers": {},
  "options": {},
  "xcom_push": True, }
 }, { 
"name": "End",
"_type": "airflow.operators.dummy_operator.DummyOperator", "parameters": {},
}, { 
"name": "ExecuteCommand",
"_type": "airflow.operators.bash_operator.BashOperator", "parameters": { "bash_command": "echo test" },
} ]
```

{ "name": "HTTPRequest "，" _ type ":" air flow . operators . http _ operator。SimpleHttpOperator "，" position": [ 520，360 ]，" parameters ":{ " http _ conn _ id ":" dummy API "，" method": "GET "，" endpoint": "/api/users/2 "，" headers": {}，" options": {}，" xcom_push": True，} }，{ "name": "End "，" _ type ":" air flow . operators . dummy _ operator。DummyOperator "，"参数":{}，}，{ "name": "ExecuteCommand "，" _ type ":" air flow . operators . bash _ operator。BashOperator "，" parameters ":{ " bash _ command ":" echo test " }，} ]

一旦我们有了操作符及其类型的表示，我们就需要解释它并生成任务。下面提到的代码将为您做到这一点。

```
def load_operator(name): 
  """Load operators dynamically""" 
  components = name.split('.') 
  mod = __import__(components[0])
  for comp in components[1:]:
     mod = getattr(mod, comp)
  return mod tasks = {}
for node in definition["nodes"]:
   operator = load_operator(node["_type"])
   params = node["parameters"]
   node_name = node["name"].replace(" ", "")
   params["task_id"] = node_name 
   params["dag"] = dag 
   tasks[node_name] = operator(**params)
```

上面的代码读取 JSON 并加载模块和操作符，然后通过从 JSON 传递参数来创建实例，并将在前面的实例中创建的 DAG 实例添加到这些任务中，以便将其链接到 DAG。然后最终将其保存在字典中以备将来使用。

# 动态创建任务关系

现在我们已经有了 DAG 对象和操作符。最后一项任务是为 DAG 中的所有任务创建关系。例如，如果我们需要一个类似下图的 DAG。

![](img/9284a98e95ca3a444fb49e000751b990.png)

在 JSON 中，我们可以使用一个对象来存储任务名称(作为键)和它们的值作为下游依赖项。

```
{ 
  "Start": ["ExecuteCommand", "HTTPRequest"],
  "HTTPRequest": ["End"],
  "ExecuteCommand": ["End"] 
}
```

既然我们有了可以表示任务关系的数据。我们可以使用下面的代码为特定的 DAG 创建这些关系。

```
for node_name, downstream_conn in definition["connections"].items(): for ds_task in downstream_conn: tasks[node_name] >> tasks[ds_task]
```

对完整代码感兴趣的人，请访问这里的。

*原载于 2021 年 4 月 1 日*[*https://rational . tech*](https://rationalize.tech/apache-airflow/how-to-dynamically-create-apache-airflow-dags-via-only-json/)*。*