# 一个强大的 Python 技巧:自定义 YAML 标签& PyYAML

> 原文：<https://medium.com/geekculture/a-powerful-python-trick-custom-yaml-tags-pyyaml-92d5635c7935?source=collection_archive---------17----------------------->

一个简单而强大的方法来改进您在 Python 中编写和使用配置文件的方式。

![](img/5737dc1905ee6a8a43ba6c171e419a95.png)

Source By author.

数据科学家或软件工程师会遇到许多需要向程序传递配置字典来配置程序如何运行的项目。通常，当完全自动化是不可能的时候，会出现这种情况，但是许多细节可以抽象为一组有限的参数。

**常见例子:**

*   设计一个具有通用和特定网站功能的 web scraper。
*   向计算集群提交作业以训练或测试机器学习模型。
*   自动化业务流程(web 操作、直运等)

当编写需要将配置从一个进程加载或传递到另一个进程的应用程序时，通常会遇到以下问题:

1.  JSON 配置的复杂解析逻辑。
2.  无法序列化和反序列化带有复杂对象的配置(像 [pickle](https://docs.python.org/3/library/pickle.html#:~:text=%E2%80%9CPickling%E2%80%9D%20is%20the%20process%20whereby,back%20into%20an%20object%20hierarchy) 这样的工具可以帮助生成一个不可读的二进制文件)。
3.  难以保护机密(访问密钥、身份验证令牌等。)在配置中

克服这些问题同时提高代码的可伸缩性和质量的一个有效方法是使用 YAML 标记和 PyYAML 来解析和加载 YAML 配置文件。

让我们跳进来吧！

# 什么是 YAML/PyYAML？

[YAML](https://yaml.org/spec/1.1/#id858600) 是一种跨语言的基于 unicode 的序列化语言，旨在帮助定义配置文件。作为 CI/CD 行业标准，您将经常看到 YAML 文件用于通过代码定义 CI/CD 管道或基础设施。

顺便说一句，这篇文章是关于 YAML(标签)的高级用法，不会涉及基本语法(见本[入门](https://learn.getgrav.org/17/advanced/yaml))。开始理解 YAML 的一个简单方法是将它与更常见的 JSON 配置文件相比较。然而，YAML 格式比 JSON 更简洁和可扩展，并且支持高级特性，比如定制标签。

PyYAML 是一个可安装的 Python 包，它实现了 YAML 1.1 解析器规范来加载和转储 YAML 文件。对本教程来说最重要的是，PyYAML 支持特定于 Python 的标记，这些标记允许您在 YAML 表示任意 Python 对象，或者从 YAML 定义构造 Python 对象。

您可以通过在命令提示符或 bash 中运行以下命令来安装 PyYAML:

```
pip install PyYAML
```

本教程结束时，您将了解如何加载 YAMl 文件和动态插入复杂的 Python 对象，以及/或者将包含复杂 Python 对象的 Python 字典写入 YAML 文件。

# 构造函数和表示函数

使用 PyYAML 的核心是构造器、表示器和标记的概念。

从高层次来看，**构造函数**允许您获取一个 YAML 节点并返回一个类实例；一个**表示器**允许你将一个类实例序列化为一个 YAML 节点；一个**标签**帮助 PyYaml 知道调用哪个构造函数或表示函数！标签使用了特殊字符**！**在标签名前标注 YAML 节点。

让我们来看一个没有任何标签的 YAML 文件的例子。

```
**example.yml (without YAML tags)
-------------------------------** name: MyBusiness
locations:
  - "Hawaii"
  - "India"
  - "Japan"
employees:
  - name: Matthew Burruss
    id: 1
  - name: John Doe
    id: 2
```

我们希望将上面的 **example.yml** 文件加载到下面的 Python 字典中:

```
class Employee:
  """Employee class."""
  def __init__(self, name, id):
    self._name, self._id = name, id

config = {
  "name": "MyBusiness",
  "locations": ["Hawaii", "India", "Japan"],
  "employees": [
    Employee("Matthew Burruss", 1),
    Employee("John Doe", 2)
  ]
}
```

在这个简单的例子中，人们可以轻松地将 YAML 加载并解析到所需的 Python 字典中，在需要的地方实例化 *Employee* 类。

然而，想象一下如果*雇员*列表在配置树中更深一层，或者如果实例化它依赖于其他复杂的 Python 对象。随着我们的配置变得越来越长，构造可能很快变得令人头痛，代码的长度、单元测试和整体复杂性也会增加！

幸运的是，PyYAML 构造函数可以提供帮助。

# 定义 PyYAML 构造函数(从 YAML 到 Python)

如前所述，构造函数允许您获取一个 YAML 节点并输出一个构造的 Python 对象。让我们看看如何定义一个构造函数来构造一个 *Employee* 对象。找到员工标签。

再次提醒一下**！**表示 YAML 的一种标记。让我们来看一个使用自定义*的新 YAML 文件！员工*标签。

```
**example.yml (with YAML tags)
---------------------------** name: MyBusiness
locations:
  - "Hawaii"
  - "India"
  - "Japan"
employees:
  - !Employee
    name: Matthew Burruss
    id: 1
  - !Employee
    name: John Doe
    id: 2
```

我们可以在下面写一个**构造函数**来解析*！运行时加载 YAML 文件时的雇员*标签。

```
**Employee PyYAML Constructor
---------------------------** import yaml

class Employee:
  """Employee class."""
  def __init__(self, name, id):
    self._name, self._id = name, id

def employee_constructor(loader: yaml.SafeLoader, node: yaml.nodes.MappingNode) -> Employee:
  """Construct an employee."""
  return Employee(**loader.construct_mapping(node))

def get_loader():
  """Add constructors to PyYAML loader."""
  loader = yaml.SafeLoader
  loader.add_constructor("!Employee", employee_constructor)
  return loader

yaml.load(open("config.yml", "rb"), Loader=get_loader())
"""
{
  'name': 'MyBusiness',
  'locations': ['Hawaii', 'India', 'Japan'],
  'employees': [
    <__main__.Employee object at 0x7f0ea2694d10>,
    <__main__.Employee object at 0x7f0ea2694d90>
  ]
}
"""
```

在上面的例子中，*！Employee* 标签定义了一个映射节点；但是，构造函数也支持标量节点。例如，如果您的 YAML 具有以下定义:

```
**greeting.yml
------------** greeting: !Greeting "world"
```

我们可以定义下面的*标量构造函数*:

```
**Scalar constructor
------------------** import yaml

def greeting_constructor(loader: yaml.SafeLoader, node: yaml.nodes.ScalarNode) -> str:
  """Construct a greeting."""
  return f"Hello {loader.construct_scalar(node)}"

def get_loader():
  """Add constructors to PyYAML loader."""
  loader = yaml.SafeLoader
  loader.add_constructor("!Greeting", greeting_constructor)
  return loader

yaml.load(open("greeting.yml", "rb"), Loader=get_loader())
"""
{
  "greeting": "Hello world"
}
"""
```

使用*标量*指导器还是*映射*构造器显然取决于用例。甚至可以定义一个[多重构造函数](https://pyyaml.org/wiki/PyYAMLDocumentation)，每当一个标签包含一个前缀时，它就实例化一个 Python 对象(尽管注意，标签不能冲突)。例如，如果您需要用单个标签实例化一组相关的类，就可以使用这种方法。

# 定义 PyYAML 表示器(从 Python 到 YAML)

既然已经介绍了*的建造者*，那我们就来看看*的代表*。一个用例可以是一个 Python 进程动态构建一个配置字典以传递给另一个 Python 进程。

为了存储配置供以后使用或修改，我们需要序列化 YAML 文件。然而，如果这个 Python 字典包含任何复杂的 Python 对象(例如，函数、类等)呢？).我们如何克服这一点？

当然是带 YAML 标签的！

假设我们想将雇员数组写回到 YAML 文件中。

```
**Example Python code
-------------------** list_of_employees = [
  Employee("Matthew Burruss", id=0),
  Employee("John Doe", id=1),
  Employee("John Doe's brother", id=2),
]
```

我们可以定义以下表示器来获取 *Employee* 实例，并使用适当的标签*将其编写为 YAML 映射节点！员工*。

```
import yaml

def employee_representer(dumper: yaml.SafeDumper, emp: Employee) -> yaml.nodes.MappingNode:
  """Represent an employee instance as a YAML mapping node."""
  return dumper.represent_mapping("!Employee", {
    "name": emp._name,
    "id": emp._id,
  })

def get_dumper():
  """Add representers to a YAML seriailizer."""
  safe_dumper = yaml.SafeDumper
  safe_dumper.add_representer(Employee, employee_representer)
  return safe_dumper

with open("output.yml", "w") as stream:
  stream.write(yaml.dump(list_of_employees, Dumper=get_dumper()))
"""
output.yml
- !Employee
  id: 0
  name: Matthew Burruss
- !Employee
  id: 1
  name: John Doe
- !Employee
  id: 2
  name: John Doe's brother
"""
```

像*构造器*一样，你也可以定义一个标量*表示器*。然而，围绕标量类型编写一个包装类通常是有用的，这样就可以将一个类类型传递给`add_representer`函数。否则，如果该标量类型(例如字符串)在 YAML 文件中的任何其他地方使用，它将不会被自定义函数表示。

既然我们已经了解了 PyYAML *构造器*、*表示器*和*标签*的基础知识，那么让我们通过几个高级示例来看看如何在实践中使用它们。

没有例子的教程算什么！接下来的部分将着眼于 YAML 标签和 PyYAML *构造函数*的具体高级用法，它们往往比*表示符*更复杂。有关示例代表，请参见[附录](https://matthewpburruss.com/post/yaml/#appendix)部分。

# 保护秘密

在 CS 项目中，我们经常有需要保护的秘密。这可能是用于从存储中加载数据集的 S3 或 ADLS 访问密钥，用于服务主体授权到服务中的客户端机密，或者用于执行 HTTP API 请求的访问令牌。

通常，服务器代码利用一个`.env`文件或一些 DevOps 特性来允许我们通过一个环境变量访问这个秘密。如果这个秘密成为我们的配置字典的一部分，如果我们想要最终将配置字典写到一个文件或者打印配置字典用于调试，这可能是一个大问题。总的来说，暴露秘密从来都不是好事！

让我们举一个简单的例子，我们想从环境中加载一个 secret *MY_SECRET* ，并在配置字典中将它设置为属性 *my_secret* 。

```
**my_secret_config.json
---------------------** {
  "my_secret": {
    "env_key": "MY_SECRET"
  }
}
```

假设我们有下面的 Python 代码。

```
**Python snippet (not good!)
--------------------------** import json
import os

# Load the config and retrieve secret from environment
config = json.load(open("my_secret_config.json", "rb"))

config["my_secret"] = os.environ[config["my_secret"]["env_key"]]  # Issue 1: Need to load each secret and be aware of the structure of the JSON

print(config) # Issue 2: Whoops! We just printed a secret!

config["my_secret"] = None  # Issue 3: We have to manually remove the secret before writing.
json.dump(config, open("out_config.json", "wb"))
```

在上面的代码中，我们可以将秘密值包装在一个 Python 类中，该类覆盖了安全打印的`__str__`方法，但是我们仍然需要注意 JSON 的结构，并在任何位置实例化秘密(这可能在配置的深处)。

如果我们需要将秘密插入到一个字符串中，比如 HTTP 访问令牌，该怎么办呢？我们需要正确格式化 JSON 配置字典，告诉我们秘密是什么以及如何格式化字符串。

相反，让我们来看看一个更干净、更可伸缩的解决方案，它使用了一个定制的`!Env` YAML 标签和一个 PyYAML 构造函数。

```
**my_secret_config.yml
--------------------** my_secret: !Env "${MY_SECRET}"
my_secret2: !Env "https://mywebsite.com?access_token=${ACCESS_TOKEN}"  # We can even insert into a string!
```

然后，我们可以编写 Python 代码来加载这个 YAML，甚至动态地将秘密插入到格式化的字符串中。在下面的例子中，我们还将这个秘密封装在一个类中，以防我们想要保护它不被打印到 stdout。

```
**!Env YAML tag and PyYAML Constructor
------------------------------------** import re
import os
import yaml

class Secret:
  def __init__(self, name, secret):
    """Initialize."""
    self._name, self._secret = name, secret
  def __str__(self):
    """Override to string method."""
    return f"Secret(name={self._name}, secret=***)"
  def get(self):
    """Get secret value."""
    return self._secret

def env_constructor(loader, node):
  """Load !Env tag"""
  value = str(loader.construct_scalar(node)) # get the string value next to !Env
  match = re.compile(".*?\\${(\\w+)}.*?").findall(value)
  if match:
    for key in match:
      value = value.replace(f'$}', os.environ[key])
    return Secret(key, value)
  return Secret(key, value)

def get_loader():
  """Get custom loaders."""
  loader = yaml.SafeLoader
  loader.add_constructor("!Env", env_constructor)
  return loader

config = yaml.load(open("my_secret_config.yml", "rb"), Loader=get_loader())
print(config)
"""
{
  'my_secret': <__main__.Secret object at 0x7f3247a75e10>,
  'my_secret2': <__main__.Secret object at 0x7f3247a75d10>
}
"""
for key, secret in config.items():
  print(f"{key}={secret}")
"""
my_secret=Secret(name=MY_SECRET, secret=***)
my_secret2=Secret(name=ACCESS_TOKEN, secret=***)
"""
```

# 替换 if-else 解析逻辑

通过在运行时用处理程序替换参数化标签，您甚至可以使用 YAML 标签来帮助减少代码中 if-else 逻辑的数量。

让我们以一个抓取工具为例，您想要通过使用下面的配置 JSON 聚合 IMDB 和 Goodreads 数据来收集哈利波特和魔法石媒体的粉丝评论:

```
**harry_potter_config.json
------------------------** [
  {
    "url": "https://www.imdb.com/title/tt0241527/",
    "provider": "imdb"
  },
  {
    "url": "https://www.goodreads.com/book/show/3.Harry_Potter_and_the_Sorcerer s_Stone",
    "provider": "goodreads"
  }
]
```

为了执行这个任务，可以编写函数来抓取 *IMDB 页面*和另一个用于 *Goodreads* 页面，加载配置 JSON，并在 if-else 块之后调用适当的处理程序。下面显示了一个示例:

```
**Python code (not good!)
-----------------------** import json
from my_scraper import handle_imdb, handle_goodreads

for scrape_config in json.load(open("harry_potter_config.json", "rb")):
  if scrape_config["provider"] == "imbd":
    handle_imdb(scrape_config["url"])
  elif scrape_config["provider"] == "goodreads":
    handle_goodreads(scrape_config["url"])
  raise ValueError(f"Unknown provider {scrape_config['provider']}")
```

虽然上面的代码可以工作，但它可能不是最好的解决方案。随着配置字典变得越来越大和越来越复杂，代码将无法伸缩，因为解析和处理所有条件变得越来越复杂。

考虑这样一种情况，一个 if-else 确定的函数将另一个 if-else 确定的函数的输出作为输入，并且该函数依赖于另一个 if-else 确定的函数的输出，以此类推。逻辑并不清晰。

虽然您可以使用一些设计模式来帮助提高解析质量(例如 *Builder* 或 *Factory* 方法),但是想象一下这种更简洁的情况，在运行时利用配置中的 YAML 标签来插入处理程序:

```
**harry_potter_config.yml
-----------------------** - url: https://www.imdb.com/title/tt0241527/
  handler: !Func
    module: my_scraper
    name: handle_imdb
- url: https://www.goodreads.com/book/show/3.Harry_Potter_and_the_Sorcerer s_Stone
  handler: !Func
    module: my_scraper
    name: handle_goodreads
```

在 Python 中，我们可以利用 [PyYAML](https://pypi.org/project/PyYAML/) 在使用`!Func`标签的地方动态插入适当的处理程序，使用映射节点告诉我们的程序加载什么函数。

```
**!Func YAML tag and PyYAML constructor
-------------------------------------** import yaml

def func_loader(loader, node):
  """A loader for functions."""
  params = loader.construct_mapping(node) # get node mappings
  module = __import__(params["module"], fromlist=[params["name"]]) # load Python module
  return getattr(module, params["name"]) # get function from module

def get_loader():
  """Return a yaml loader."""
  loader = yaml.SafeLoader
  loader.add_constructor("!Func", func_loader)
  return loader

config = yaml.load(open("harry_potter_config.yml", "rb"), Loader=get_loader())
for scrape_config in config:
  scrape_config["handler"](scrape_config["url"])

"""
config
[
  {
    'url': 'https://www.imdb.com/title/tt0241527/',
    'handler': <function handle_imdb at 0x7f3247a76320>
  },
  {
    'url': 'https://www.goodreads.com/book/show/3.Harry_Potter_and_the_Sorcerer s_Stone',
    'handler': <function handle_goodreads at 0x7f3247a764d0>
  }
]
"""
```

`yaml.load`的返回值包含一个 Python 字典，其中函数处理程序 *handle_imdb* 和 *handle_goodreads* 加载到 *handler* 属性，提高了代码的可伸缩性和可维护性。

前面还提到，随着配置字典变得越来越大和越来越复杂，解析逻辑也是如此。YAML 也可以帮助解决这个问题，因为它可以解决字典中任何地方的依赖性。下面我们来看一个例子。

```
**deep_config.yml
---------------** class_a: !MyClass
  param1: 1
  order: 1
class_b: !MyClass
    param1: !MyClass
      param1: 2
      order: 2
    order: 3
```

然后，像之前一样，我们可以定义一个 PyYAML 构造函数并检索加载器。

```
**!MyClass tag and PyYAML constructor
-----------------------------------** import yaml

class MyClass:
  def __init__(self, param1, order):
    print(f"Initializing with name={param1} order={order}")

def my_class_loader(loader, node):
  """A loader for functions."""
  return MyClass(**loader.construct_mapping(node))

def get_loader():
  """Return a yaml loader."""
  loader = yaml.SafeLoader
  loader.add_constructor("!MyClass", my_class_loader)
  return loader

yaml.load(open("deep_config.yml", "rb"), Loader=get_loader())
"""
Initializing with name=1 order=1
Initializing with name=2 order=2
Initializing with name=<__main__.MyClass object at 0x7f3247a7aa50> order=3
{
  'class_a': <__main__.MyClass object at 0x7f3247a7a710>,
  'class_b': <__main__.MyClass object at 0x7f3247a7a750>
}
"""
```

# 结论

教程到此结束！下次您编写 Python 程序时，如果发现需要配置字典来提高代码的可重用性和功能性，您可以利用 YAML 和 PyYAML 来减少解析开销，并帮助序列化/反序列化配置字典。

任何问题或意见，请在下面留下。感谢阅读！

# 附录

本教程中高级示例的 PyYAML 表示如下所示。

```
**Representer for !Env
--------------------** def _env_representer(dumper, env):
  return dumper.represent_scalar("!Env", env.secret)**Representer for !Func
---------------------** def _func_representer(dumper, func):
  return dumper.represent_mapping("!Func", {
    "module": func.__module__,
    "name": func.__name__
  })
```

*原载于 2021 年 4 月 24 日*[*【https://matthewpburruss.com】*](https://matthewpburruss.com/post/yaml/)*。*