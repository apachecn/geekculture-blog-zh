# 学习 Django: REST 框架和 MVT 架构

> 原文：<https://medium.com/geekculture/learning-django-rest-framework-and-mvt-architecture-1ca173163f1c?source=collection_archive---------14----------------------->

*定义 Django REST 框架(DRF)，Django 的 MVT 设计模式，以及它与更典型的 MVC 架构的比较*

![](img/6f61079e0ab55f0367f6dcc76fa8ca4d.png)

Photo by [Faisal](https://unsplash.com/@faisaldada?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

本周，为了试验一个新的后端框架，我决定在后端构建一个由 Django 支持的常用待办事项应用程序，并在前端做出反应。在这个过程中，我开始更多地了解 Django REST 框架，这是一个强大的工具包，用于构建服务器端所需的 Web APIs，以及它的模型-视图-模板设计架构。

## 姜戈是什么？

在深入一些与用 Django 构建 Web APIs 相关的核心概念之前，让我们先定义一下 Django 是什么，以及它对开发人员有什么用。

**Django** 是一个 Python web 框架，它简化了 web 开发中的常见实践，通过提供一个支持常见开发需求的稳定库生态系统来解决许多麻烦。

## 什么是休息？

REST，或**表述性状态转移**，是一种在计算机系统之间提供标准的架构风格。

MDN 网络文档[是这样解释](https://developer.mozilla.org/en-US/docs/Glossary/REST)的:

> REST 的基本思想是，一个资源，例如一个文档，是通过公认的、与语言无关的、可靠的标准化客户机/服务器交互来传输的。当服务遵守这些约束时，它们被认为是 RESTful 的。

遵循这种风格，前端/客户端和后端/服务器端的代码可以相互独立地实现，而不需要改变相反服务的操作。不同的用户可以访问相同的 REST 端点，执行相同的操作，并获得相同的响应。

在 REST 框架下，客户端或浏览器发送一个检索或编辑数据的请求，服务器接收请求并发回一个响应。用于与 REST 系统中的数据点/资源交互的 4 个 HTTP 动词，定义了要执行的操作，包括: **GET、POST(创建新资源)、PATCH/PUT(编辑资源)、DELETE** 。

一个 **REST API** (也称为 RESTful API)是一个符合 REST 架构风格约束的应用编程接口(API)。在这些 API 中，路径被设计成帮助客户理解由于他们的请求而发生的事情(例如，定位特定的数据/资源)。

浏览器/用户通过 RESTful API 发出请求后，它会将资源状态的表示传递给请求者。这些信息通过 HTTP 以几种格式中的一种传递，最常见的是 JSON (Javascript Object Notation)。

## Django REST 框架(DRF)

从头开始创建 REST APIs 需要时间和精力，但是 Django 提供了一个可安装的扩展，它提供了更快开始创建 API 的功能。

# DRF 装置

在启动并运行 Django 项目和应用程序之后(这个七部分教程系列的第一部分[和第二部分](https://docs.djangoproject.com/en/3.2/intro/tutorial01/)[为初学者提供了一个坚实的基础)，在 Django 虚拟环境中运行以下命令，开始构建 Web APIs:](https://docs.djangoproject.com/en/3.2/intro/tutorial02/)

```
pipenv install djangorestframework django-cors-headers
```

一旦你从命令行安装了它，将`‘corsheaders’`和‘`rest_framework’`添加到包含的`settings.py`文件的`Installed_Apps`列表中。

# Django 的设计模式

我假设 Python 框架将遵循 MVC——模型、视图、控制器——软件设计模式，因为它通常在 Ruby、Java、C、C++等语言中使用。尽管基本的逻辑适用，Django 代替了 MVT——模型、视图、模板——架构，基于一些值得探索和理解的关键差异。

拥有像 MVC 或 MVT 这样的设计架构强调将数据表示从交互和处理数据的组件中分离出来。

**模型** —应用程序中数据的接口，它提供了整个应用程序的逻辑结构，由数据库表示(通常是 Postgres 或 MYSQL 之类的关系数据库)。

**视图** —接受来自浏览器的 HTTP 请求并返回 HTTP 响应。它充当模型和模板之间的桥梁，基于从数据库中检索适当的数据来呈现模板。

**模板**—HTML 代码。与 React 这样的前端框架/库无关。在 MVC 模式中，模板与视图最为相似。

与 MVC 模式相比，Django 框架本身处理 MVC 模式中控制器**负责的事情。**

MVT 流程的这一步[解释](https://www.askpython.com/django/django-mvt-architecture)很好地解释了这一点:

> 1.用户向 Django 发送一个对资源的 URL 请求。
> 
> 2.Django 框架然后搜索 URL 资源。
> 
> 3.如果 URL 路径链接到一个视图，那么这个特定的视图被调用。
> 
> 4.然后，视图将与模型进行交互，并从数据库中检索适当的数据。
> 
> 5.然后，视图将适当的模板和检索到的数据一起呈现给用户。