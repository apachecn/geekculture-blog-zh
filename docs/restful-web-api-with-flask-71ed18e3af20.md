# 带 Flask 的 RESTful Web API

> 原文：<https://medium.com/geekculture/restful-web-api-with-flask-71ed18e3af20?source=collection_archive---------13----------------------->

![](img/65acaed2eed515e67d947c025785089f.png)

# 介绍

Flask 使得创建 RESTful web API 变得非常容易。已知的 route() decorator 除了它的方法可选参数之外，还可以用来声明路由。它控制服务公开的资源 URL。JSON 数据工作太简单了。因为包含在请求中的 JSON 数据被自动公开为 request.json Python 字典。需要包含 JSON 的响应可以使用 Flask 的 jsonify() helper 函数从 Python 字典中创建。

在本文中，我们将学习如何用 Flask 创建一个 RESTful web API。

# 描述

Flask-RESTful 是 Flask 的扩展。它为快速构建[REST API 提供了帮助。](https://www.technologiesinindustry4.com/2021/12/restful-web-services.html)这是一个轻量级的抽象，可以与现有的库一起工作。Flask-RESTful 以最少的设置支持最佳实践。如果我们熟悉 Flask，Flask-RESTful 应该很容易上手。

# 装置

使用 pip 安装[烧瓶-RESTful](https://www.technologiesinindustry4.com/2021/12/restful-web-services.html)

```
pip install flask-restful
```

# 一个最小的烧瓶应用

```
from flask import Flaskapp = Flask(__name__)@app.route('/hello/', methods=['GET', 'POST'])
def welcome():
    return "Hello World!"if __name__ == '__main__':
    app.run(host='0.0.0.0', port=105)
```

*   将它保存为 api.py，并使用 python 解释器运行它。
*   转到终端并键入`python app.py`(即`python <filename>.py`)

我们会看到这样的情况:

```
Running on [http://0.0.0.0:105](http://0.0.0.0:105)
Press CTRL+C to quit
```

*   启动任何网络浏览器。
*   要查看应用程序的运行情况，请访问`http://localhost:105/hello/`。

**代码逐行工作**

*   将烧瓶类导入为`from flask import Flask`
*   创建一个类的实例作为`app = Flask(__name__)`
*   我们使用`route()`装饰器来询问 Flask 什么 URL 应该触发这个函数。
*   `@app.route('/hello/', methods=['GET', 'POST'])`
*   `methods`表示允许哪些 HTTP 方法。默认为`['GET']`
*   `if __name__ == '__main__'` → `__name__`是 Python 中的特定变量。
*   它接受脚本名的值。
*   这一行确保我们的 Flask 应用程序只有在主文件中执行时才运行。
*   当它被导入到其他文件中时，将不会被执行。
*   运行烧瓶应用程序`app.run(host='0.0.0.0', port=105)`
*   `host`表示我们希望 flask 应用程序运行的服务器。
*   `host`的默认值是`localhost`和`127.0.0.1`
*   `0.0.0.0`显示为*本地机器上的所有 IPv4 地址。*
*   这确保了从所有地址都可以到达服务器。
*   默认的`port`值是`5000.`
*   我们可以设置参数`port`来使用我们选择的端口号。

# 变量规则

*   我们可以通过使用`<variable_name>`向 URL 添加可变部分。
*   该函数获取变量作为关键字参数。

```
from flask import Flask
app = Flask(__name__)@app.route('/<int:number>/')
def incrementer(number):
    return "Incremented number is " + str(number+1)@app.route('/<string:name>/')
def hello(name):
    return "Hello " + nameapp.run()
```

*   要启动 Flask 应用程序，请运行上面的代码。
*   打开浏览器并转到`[http://localhost:5000/Jimit.](http://localhost:5000/Jimit.)`
*   我们将输出视为`Hello Jimit`。
*   当我们转到`http://localhost:5000/10`时，输出将是`Incremented number is 11`。

# 创建 API 蓝图

*   与 RESTful API[链接的路由构成了应用程序的一个独立子集。](https://www.technologiesinindustry4.com/2021/12/restful-web-services.html)
*   因此，将它们放在自己的蓝图中是保持它们井井有条的最好方法。
*   下面的例子显示了应用程序中 API 蓝图的通用结构。

```
|-flaky
|-app/
|-api_1_0
|-__init__.py
|-user.py
|-post.py
|-comment.py
|-authentication.py
|-errors.py
|-decorators.py
```

*   请注意用于 API 的包是如何在其名称中添加版本号的。
*   它可以作为具有不同版本号的另一个包来包含，并且当需要引入 API 的向后不兼容版本时，可以同时提供这两个 API。
*   这个 API 蓝图在单独的模块中应用每个资源。
*   类似地添加模块来处理认证、错误处理，并提供定制的装饰器。
*   蓝图构造器可以在下面的例子中看到。

```
from flask import Blueprint
api = Blueprint('api', __name__)
from . import authentication, posts, users, comments, errors
```

API 蓝图的注册可以在下面的例子中看到。

```
def create_app(config_name):
# ...
from .api_1_0 import api as api_1_0_blueprint
app.register_blueprint(api_1_0_blueprint, url_prefix='/api/v1.0')
# ...
```

更多详情请访问:[https://www . technologiesinindustry 4 . com/2022/01/restful-we b-API-with-flask . html](https://www.technologiesinindustry4.com/2022/01/restful-web-api-with-flask.html)