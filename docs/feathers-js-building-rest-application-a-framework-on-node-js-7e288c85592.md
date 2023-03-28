# Feathers.js—构建 REST 应用程序(Node.js 上的一个框架)

> 原文：<https://medium.com/geekculture/feathers-js-building-rest-application-a-framework-on-node-js-7e288c85592?source=collection_archive---------4----------------------->

![](img/c624df31bde743a8ab64d2dce83ed0d3.png)

***→ Feathers.js*** *是一个用于 NodeJS* 的实时微服务 web 框架，它通过 RESTful 资源、套接字和灵活的插件让您控制您的数据。

→应用程序可以用 JavaScript 或 TypeScript 创建

→ Feathers 可以与**任何后端技术**交互，支持十几个数据库，并与 **React、VueJS、Angular、React Native、Android 或 iOS 等任何前端技术协同工作。**

**羽毛的重要特征 sJS:**

1.  FeathersJS 是作为 NodeJS 的微服务框架开发的，也可以是 web 应用程序
2.  支持关系数据库和非关系数据库
3.  简化实时应用程序开发
4.  简化面向服务的应用的开发
5.  自动提供 REST APIs
6.  支持身份验证
7.  加速错误处理

# 我们将理解的是:

在本文结束时，您将对以下内容有一个很好的理解:

a.使用 JWT 创建身份验证

b.CRUD 操作使用 Feathers.js 框架的节点

c.失眠

# 安装:

```
npm install @feathersjs/feathers --savenpm install -g @feathersjs/cli
```

→安装 feathers 后，打开终端或命令提示符，输入以下命令

```
feathers --help
```

![](img/9908db4360f6008cc9027edb914f057d.png)

→我们可以看到 feathers 提供的一系列命令——应用、认证、连接、挂钩、中间件、服务、插件

# **我们将开发什么:**

Feathers 是一个 Node.js 框架，将处理大多数后端 js 开发

**前端:**我们会用失眠/邮差。我们可以使用任何前端 JS 框架，如 React.js、Angular.js 或 Vue.js

**数据库:** NeDB，该数据库的特性它将数据存储在**内存**中。我们也可以使用任何关系数据库，如 Postgres、MongoDB 和非 SQL 数据库，如 MongoDB。

# **创建新应用:**

```
feathers g app
```

→在终端中，它会询问一些问题——通过 JavaScript 或 TypeScript 创建:让我们为这个应用程序选择 JavaScript

![](img/447ae52cba72c39a34d161428fc8943c.png)

→我们将根据项目要求回答一系列问题。选择了和下面一样的

![](img/3acabca6cd480f1a84243ce32db96c38.png)

→有多种**认证类型**功能可用——用户名+密码、Auth0、谷歌、脸书、Twitter 和 GitHub。

使用空格按钮选择多种身份验证方法。

我们可以根据客户的要求选择。为了简单起见，我们将选择**用户名+密码**

→选择 DB 为 **NeDB** 。同样，根据需求，我们选择了任何数据库

![](img/a5450db3521ee0b245a549b910faa0a5.png)

→最后，它将按照我们的配置安装样板文件

![](img/deed011e621c532eac6aa734b19e4208.png)![](img/7c20ba0fa16a3424eeff67d2f1351a05.png)

**目录结构:**

![](img/aec57e2c305335d712e4d2cf76a10737.png)

→使用以下命令启动应用程序:

```
yarn dev
npm run dev
```

![](img/a3104e3dac48e5def76d5a18e90b7751.png)

→应用程序在 [http://localhost:3030](http://localhost:3030) 中打开

![](img/46289342789c0319be2b3184b8de771b.png)

# 失眠:

→我们将使用失眠来测试我们的 REST APIs。在这里下载。

→这个类似于邮递员。我也想更多地了解这个 API 客户端。所以用了这个😄

![](img/b99d08dab0abb624324913e6646ba4ba.png)![](img/881998d899c4d39b612a64b1c8155624.png)![](img/e36d898815624d0910b8cac9c7bf7a73.png)

→我们将在该集合的环境变量中设置**基本 URL** 。

→打开 **Cmd + E (Mac)或 Ctrl + E (Windows)** 设置基本变量。这是键值设置

```
baseUrl: 'http://localhost:3030'
```

![](img/ba13df5733b098d8c6430da634ab40e8.png)![](img/36632d3affca13b7784fde77199fcd6c.png)

→让我们创建第一条路线:

![](img/7f44e983f060b0b83cfd023b9ee5ea0b.png)

→写入 baseUrl 时，将从环境变量加载值

![](img/8a1176c107ffb2cdbf218cb32e09a978.png)![](img/b6b2eb5f12cb1eafb36d83f2ff262220.png)![](img/7e5f0a94b47d1675186ddd681795cae5.png)

→我们得到了一些回应。是的，现在是 404😄。

→我们的失眠起作用了。让我们创造一些有意义的路线。

→我们很高兴知道 Feathers 具有自动创建的 REST 功能。我们将只使用正确的路线，并获取或发布我们的数据。

# **认证:**

→ Feathers 使用 JWT 令牌概念创建会话。

→我们使用用户名+本地认证(其类型为本地)。根据需要，可以有更多的身份验证类型。

→我们称之为用户端点。

> 注意:调用任何端点都会首先调用用户钩子

![](img/22c72495ddf7906c25472202c74075a3.png)

1.  **注册(创建操作):**

> 类型:邮政
> 
> 网址: [http://3030/users](http://3030/users)

请求 JSON 正文:

```
{
 "name":"Amir Mustafa",
 "email": "[amir.mustafa@gmail.com](mailto:amir.mustafa@gmail.com)",
 "password": "amir@123"
}
```

回应:

```
{
  "name": "Amir Mustafa",
  "email": "[amir.mustafa@gmail.com](mailto:amir.mustafa2@gmail.com)",
  "_id": "vUD8zJdDQQsCBLsQ"
}
```

![](img/7006b764cbb031ca81375840eebe738f.png)

条目 2:

![](img/7006b764cbb031ca81375840eebe738f.png)

→数据保存在内存中的 **NeDB** 中(即保存在 **data/*** 目录中)。用于在 **data/users.db** 文件中注册

![](img/b6e96b8dfaef1c5b192bda2031d7d15a.png)

**2。登录:**

我们需要在失眠 API 客户端中传递以下细节。

> 类型:邮政
> 
> 网址:[http://localhost:3030/authentic ation](http://localhost:3030/authentication)

请求 JSON 正文:

```
{ "email": "raj.khanna@gmail.com", "password": "raj@123", "strategy": "local"}
```

回应:

```
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6ImFjY2VzcyJ9.eyJpYXQiOjE2MzMxMTEwNjcsImV4cCI6MTYzMzE5NzQ2NywiYXVkIjoiaHR0cHM6Ly95b3VyZG9tYWluLmNvbSIsImlzcyI6ImZlYXRoZXJzIiwic3ViIjoiQTJjWG1SUXV1THRYcnh0aSIsImp0aSI6IjliNGIxMzhmLWMxODEtNDY4Ni04MzViLTQyNzU5Y2JiMDhkZiJ9.k_Uazv4-NLz5WiuziOlt9D8yeUeCLG5fhc2RzmTVN1Q", "authentication": {
    "strategy": "local",
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6ImFjY2VzcyJ9.eyJpYXQiOjE2MzMxMTEwNjcsImV4cCI6MTYzMzE5NzQ2NywiYXVkIjoiaHR0cHM6Ly95b3VyZG9tYWluLmNvbSIsImlzcyI6ImZlYXRoZXJzIiwic3ViIjoiQTJjWG1SUXV1THRYcnh0aSIsImp0aSI6IjliNGIxMzhmLWMxODEtNDY4Ni04MzViLTQyNzU5Y2JiMDhkZiJ9.k_Uazv4-NLz5WiuziOlt9D8yeUeCLG5fhc2RzmTVN1Q",
    "payload": {
      "iat": 1633111067,
      "exp": 1633197467,
      "aud": "[https://yourdomain.com](https://yourdomain.com)",
      "iss": "feathers",
      "sub": "A2cXmRQuuLtXrxti",
      "jti": "9b4b138f-c181-4686-835b-42759cbb08df"
    }
  },
  "user": {
    "name": "Amir Mustafa",
    "email": "[amir.mustafa@gmail.com](mailto:amir.mustafa@gmail.com)",
    "_id": "A2cXmRQuuLtXrxti"
  }
}
```

![](img/9e80f3fd89e6349235e20aac0011d486.png)

→我们成功获得了登录响应🙂

# CRUD 操作:

→ CRUD 代表创建、读取、更新和删除操作。

→在所有路由中，从登录接收的 **accessToken** 是必须的，由 feathers 自动处理

→让我们将其添加到环境变量中:

→我们将在该集合的环境变量中设置**标记**。

打开 **Cmd + E (Mac)或 Ctrl + E (Windows)** 设置基本变量。这是键值设置

![](img/c4129074690b4ca99f4b46fce1d67b1e.png)

→现在对于 CRUD，让我们在同一个应用程序中创建一个新的服务车辆。

→要创建新服务，我们使用以下命令:

```
feathers g service
```

![](img/f4c8c145fbabfce88f2e000a71075746.png)

→我们将遵循与创建应用程序时相同的步骤，唯一不同的是**名称(车辆)和路径(/车辆)(参考上面的截图)。**

→让我们检查应用程序中的目录结构:

![](img/8ebe711f0cc03ffb6f1d9e78ed203a6a.png)

→ REST 代码再次被自动处理。让我们去失眠和执行我们的路线

1.  **创建车辆:**

让我们为车辆服务创建一个新的目录，我们将在其中保存所有的车辆路线

> 类型:邮政
> 
> 网址:[http://localhost:3000/vehicles](http://localhost:3000/vehicles)

请求:

```
{
 "brand": "TATA",
 "model": "Grand i10",
 "description": "Another success TATA product",
 "price": 1500000
}
```

选择验证类型:不记名令牌

![](img/2e303bdde09ec03ec4ab5ca3656e73a8.png)![](img/04a1eb223f0634e9729b96acebca06ca.png)

回应:

```
{
  "brand": "TATA",
  "model": "Grand i10",
  "description": "Another success TATA product",
  "price": 1500000,
  "_id": "W05so7ulQWO3RCmG"
}
```

并且数据被插入

条目 2:

![](img/029dff6fe5b7c8f90651c6dba6470a87.png)

条目 3:

![](img/f1ca7c5f86cb32900036c2c086d4e176.png)

→所有数据都存储在 NeDB 存储器中。让我们签入代码

![](img/cd85f5636d98e1aa40569d147af6d40b.png)

**2。读取车辆:**

→我们可以读取车辆中所有插入的数据。

→这可以是获取完整数据，也可以是基于参数的过滤数据

**答:读取所有数据**

> 类型:获取
> 
> 网址:[http://localhost:3030/vehicles](http://localhost:3030/vehicles)

请求:没有对 GET 请求的请求调用

回应:

```
{
  "total": 4,
  "limit": 10,
  "skip": 0,
  "data": [
    {
      "brand": "Tesla",
      "model": "X",
      "description": "Awesome electric car",
      "price": 1000000,
      "_id": "EJP1d7vnKm5RmcFZ"
    },
    {
      "brand": "TATA",
      "model": "Grand i10",
      "description": "Another success TATA product",
      "price": 1500000,
      "_id": "IiVncDRAXcNAKCD9"
    },
    {
      "brand": "TATA",
      "model": "i20",
      "description": "Another success TATA product",
      "price": 1500000,
      "_id": "UT3Fn2rXqsVQ3DEN"
    },
    {
      "brand": "Tesla",
      "model": "Mahindra",
      "description": "Great Electric Car",
      "price": 2500000,
      "_id": "ZiJRcD0U3EHehryc"
    }
  ]
}
```

→我们还获得了自动分页处理的数据(参见 config/default.json)。

→不会为此传递车身数据。传递令牌必须作为 Auth: Bearer 令牌

![](img/581b5d17a552129e126978004677c557.png)

**B .读取过滤后的数据:**

> 类型:获取
> 
> 网址:[http://localhost:3030/vehicle](http://localhost:3030/vehicles)s？brand =<filter _ parameter>

例:[http://localhost:3030/vehicle](http://localhost:3030/vehicles)s？品牌=塔塔

请求:没有对 GET 请求的请求调用

回应:

```
{
  "total": 2,
  "limit": 10,
  "skip": 0,
  "data": [
    {
      "brand": "TATA",
      "model": "Grand i10",
      "description": "Another success TATA product",
      "price": 1500000,
      "_id": "IiVncDRAXcNAKCD9"
    },
    {
      "brand": "TATA",
      "model": "i20",
      "description": "Another success TATA product",
      "price": 1500000,
      "_id": "UT3Fn2rXqsVQ3DEN"
    }
  ]
}
```

![](img/b6234903b646ca2575f09b426681c8bd.png)

**3。更新车辆:**

→您可以转到 NeDB 文件(即 data/vehicles.db)并在那里更新数据

→更新您的数据。重启节点服务器(即 yarn dev)。全部完成

```
yarn dev or
npm run dev
```

![](img/bb6c41821913f15551bf6dc7cfbb127f.png)

更新后:

![](img/87c08d65d05602be22ac7aec98dee895.png)

→通过读取操作，我们可以看到数据已更新。

![](img/ce8e7e19ef952c0d7ec200d6496446c0.png)

**4。删除操作:**

→我们需要进入 NeDB 服务文件(即 data/vehicles.db)并删除所需数据，如下图所示。

→重启节点服务器

```
yarn dev or
npm run dev
```

![](img/1bb5fba0b76953b797ae4d8e67c4b41b.png)![](img/328d763d579351f9ea056e23c51cb389.png)![](img/1d98a6fe8bcb668d4cd6e6d8e6b70ca9.png)

代码库:

[](https://github.com/AmirMustafa/feathers-REST-API) [## GitHub-Amir Mustafa/feathers-REST-API:使用 Feathers.js 创建 REST API(构建在顶层的框架…

### Feathers.js 是一个用于 NodeJS 的实时、微服务开源 web 框架，让您可以控制自己的数据…

github.com](https://github.com/AmirMustafa/feathers-REST-API) 

# 结束语:

我们已经在很短的时间内创建了一个安全的 REST 应用程序。羽毛省去了我们一两天的工作。它支持多个数据库和前端。

除了 REST，feathers 还有更多特性，比如 sockets，多重单点登录认证，比如脸书，GitHub 等等。

> 谢谢你一直坚持到最后🙌。如果你喜欢这篇文章或者学到了新的东西，请点击下面的分享按钮来支持我，让更多的人了解我和/或在 [Twitter](https://twitter.com/amir__mustafa) 上关注我，看看我在那里学到和分享的其他技巧、文章和东西。