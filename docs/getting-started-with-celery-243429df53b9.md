# 芹菜和兔肉入门

> 原文：<https://medium.com/geekculture/getting-started-with-celery-243429df53b9?source=collection_archive---------5----------------------->

![](img/1f4528104f967d93ad3f58dc083f95c5.png)

Getting Started with Celery

在这篇文章中，我将涵盖芹菜的基本概念。

*   什么是任务队列？
*   芹菜介绍
*   什么是经纪人？
*   选择经纪人
*   安装芹菜
*   处理芹菜
*   结论

让我们开始吧…

1.  **什么是任务队列？**

*   任务**的**就是**的**本质上是一个 python 函数；给定一组参数就可以完成的任务。任务队列是一组要运行的任务。
*   一个应用程序中可以定义多个命名队列。任务队列允许应用程序在用户请求之外异步执行工作，称为任务。
*   如果应用程序需要在后台执行工作，它会将任务添加到任务队列中。这些任务稍后由工作服务执行。任务队列服务是为异步工作而设计的。

**2。芹菜简介**

*   Celery 是一个开源的异步任务队列或作业队列，它基于 Python web 应用程序的分布式消息传递，用于在 HTTP 请求-响应周期之外异步执行工作。
*   Celery 可以在单台机器上运行，也可以在多台机器上运行，甚至可以跨数据中心运行。
*   Celery 通过消息进行通信，通常使用一个代理在客户和工人之间进行调解。为了启动一个任务，客户机向队列中添加一条消息，然后代理将这条消息传递给一个工人。
*   芹菜简单、快速、灵活，而且可用性很高。
*   Celery 很容易与 web 框架集成，如 [Django](https://www.djangoproject.com/) 、 [Flask](https://flask.palletsprojects.com/) 等。

**3。什么是经纪人？**

*   经纪人是买方和卖方之间的第三方调解人。
*   芹菜需要一个发送和接收消息的解决方案；通常，这是以称为*消息代理*的独立服务的形式出现的。
*   在芹菜，经纪人是[雷迪斯](https://redis.io/)、 [RabbitMQ](https://www.rabbitmq.com/) 等在客户和芹菜之间传递消息的人。

**4。选择经纪人**

*   在本例中，我使用的是 [RabbitMQ](https://www.rabbitmq.com/) 。
*   RabbitMQ 功能齐全，稳定，耐用，易于安装。这是生产环境的绝佳选择。
*   您可以使用以下命令安装和设置 [RabbitMQ](http://www.rabbitmq.com/) :

```
sudo apt-get install rabbitmq-server
```

**5。安装芹菜**

Celery 在 Python 包索引(PyPI)上，所以可以用标准的 Python 工具安装，如`pip`或`easy_install`

```
pip install celery
```

**6。处理芹菜**

*   创建文件`tasks.py`

```
**from** **celery** **import** Celery

app = Celery('tasks', broker='pyamqp://guest@localhost//')

**@app**.task
**def** **add**(x, y):
    **return** x + y
```

*   现在，您可以通过使用`worker`参数执行我们的程序来运行 worker:

```
celery -A tasks worker --loglevel=INFO
```

*   要获得帮助，您可以使用以下命令:

```
celery worker --help
celery help
```

*   让我们创建一个文件 test.py 来调用我们的任务

```
**from** **tasks** **import** addresult = add.delay(**4**, **4**)
#delay() is used to call a taskprint(result.ready())
#ready() returns whether the task has finished processing or not.print(result.get(timeout=**1**))
#get() is used for getting resultsresult.get(propagate=**False**)
#In case the task raised an exception, **get()** will re-raise the exception, but you can override this by specifying the propagate argument
```

**7。结论**

*   Celery 是在 python 中执行异步任务的一个很好的选择。它是分布式的，易于使用。
*   Celery 帮助我们减轻了应用服务器的负担，这有助于我们更快地满足您的请求。

感谢阅读。如果你发现这篇文章有用，别忘了与你的朋友和同事分享。:)如果你有任何问题，请随时联系我。
**与我接通👉**[**LinkedIn**](https://linkedin.com/in/hiteshmishra708)**[**Github**](https://github.com/hiteshmishra708)**:)****