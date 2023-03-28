# JavaScript 中的 Winston Logger

> 原文：<https://medium.com/geekculture/winston-logger-in-javascript-16d54dbe931f?source=collection_archive---------3----------------------->

当您需要对 Node.js 应用程序中的问题进行故障排除时，日志可以帮助您找出问题的严重性以及导致问题的原因。可以在日志中捕获堆栈跟踪和其他类型的活动，并追溯到特定的会话 id、用户 id、请求端点——任何有助于您更有效地监控应用程序的东西。有了 **console.log** ，Node.js 有了内置的日志功能，但是像 **Winston** 这样的专用日志库给了你更多编写应用程序日志的选择。

![](img/b07dd35847f62ba23b5b9fac3e6613a2.png)

在本指南中，我们将讨论如何获取、定制和丰富您的节点。温斯顿登录 js

Winston 是一个流行的 Node.js 日志包，具有很大的灵活性。它有很多选项可以定制日志的元数据和结构，以及如何处理和存储它们。

首先，如果您还没有安装，您需要安装 Winston:

```
npm install --save winston
```

创建一个日志配置文件(例如 logger.js ),您的应用程序代码将导入该文件以便开始使用。虽然 Winston 模块提供了一个默认的日志记录器，但是开发自己的日志记录器可以让您更好地控制日志格式、异常处理以及日志的交付位置(例如，控制台、文件或流)。在日志文件中为您的应用程序创建一个日志记录器，并将其配置为将日志输出到目的地(也称为传输):

```
**logger.js**const { createLogger,transports} = require('winston');

const logger = createLogger({
   transports: [
       new transports.Console()
     ]
 });
 module.exports = logger;
```

上面的示例显示了一个简单的记录器，您可以立即在您的应用程序中使用它。Winston 要求每个测井仪至少有一个运输工具，而许多**运输工具**都可以使用。在构建应用程序和基础设施时，为应用程序的每个关键组件建立和设置不同的日志记录程序是一个很好的实践，这样您就可以立即识别日志来自哪里。在调试问题时，这允许您快速定位适当的服务或功能。为此，为不同的应用程序服务创建不同的日志:

```
[...]
const userLogger = createLogger({
   transports: [
       new transports.Console()
     ]
});
const paymentLogger = createLogger({
   transports: [
       new transports.Console()
     ]
});

module.exports = {
 userLogger: userLogger,
 paymentLogger: paymentLogger
};
```

这允许您根据应用程序服务调整每个记录器的配置。然后，在您的应用程序代码中，您可以通过导入 logger.js 配置并调用它来利用特定的记录器(例如，userLogger、paymentLogger ):

**例如:-**

从 user.js 调用记录器

```
**/routes/users.js**const {userLogger, paymentLogger} = require('./logger');
*// require logger.js*[...]
userLogger.info('New user created');
```

记录器将记录到控制台

```
{"message":"New user created.","level":"info"}
```

# 日志级别

它显示问题的严重性，有助于应用程序活动的分类。例如，信息日志通知您标准的应用程序活动，如连接到数据库，而错误日志指示您的应用程序有问题。默认情况下，Winston 使用 npm 日志优先级协议，该协议将日志记录级别的优先级从 0(最高严重程度)到 5(最低严重程度)

```
0: error 
1: warn
2: info
3: verbose
4: debug
5: silly
```

# 记录到文件中

尽管 Winston 和其他日志库提供了多种存储日志的方法，但是对于生成大量日志的复杂应用程序或系统来说，将日志记录到文件中更好。这一最佳实践确保了日志的副本始终在服务器上本地维护。这还意味着，如果您的应用程序服务器由于网络相关的故障而无法将日志流式传输到您的传输，您也不会失去重要的可见性。通过记录到一个或多个文件，还可以轻松设置日志管理服务来实时跟踪日志文件，使您能够分析日志文件，并将其与来自单个平台上其余基础架构和应用的监控数据相关联。

您可以将记录器配置为将日志输出到文件，方法是将其添加为传输:

```
**logger.js**const userLogger = createLogger({
   levels: config.syslog.levels,
   transports: [
       new transports.Console(),
       new transports.File({ filename: 'combined.log' })
     ]
 });
```

从上面的例子来看。你的日志将记录到控制台以及一个**组合. log** 文件中。这确保您可以在本地调试问题时检查日志，并轻松地将它们发送到日志管理提供程序。

# **日志的全局元数据**

![](img/37313f145194564065b81d47a706b940.png)

在将日志集中到日志管理平台之后，全局级别的元数据对于搜索和分析它们，以及识别与单个日志记录器相关联的应用程序服务可能很有价值。要将记录器配置为向它生成的每个日志提供全局元数据，请使用 Winston 的默认元参数:

```
**logger.js**const userLogger = createLogger({
   levels: config.syslog.levels,
   defaultMeta: { component: 'user-service' },
   transports: [
       new transports.Console(),
       new transports.File({ filename: 'combined.log' })
     ]
 });
```

在上面的例子中，记录器会自动在所有日志中包含一个`component`和属性:

```
{"component":"user-service","level":"info","message":"Session connected"}
```

## 注意:单个日志可以直接添加元数据。

```
**/routes/users.js***//require logger.js* const {userLogger, paymentLogger} = require('./logger');

userLogger.info('Session connected', { sessionID: `${req.id}` });
```

您的记录器会自动将`sessionID`作为新属性添加到您的日志中:

```
{"component":"user-service","level":"info","message":"Session connected","sessionID":"ak6xayY_UENoqJqXAAAA"}
```

# 你的日志格式

Winston 包含用于定制 JSON 日志和控制日志在传输中的显示方式的内置格式。这允许您使用单一格式、组合多种形式或构建自己的格式。为了给 JSON 格式的日志添加时间戳，这个例子结合了 Winston 的时间戳和 JSON 格式:

```
**logger.js**const { createLogger, format, transports, config } = require('winston');
const { combine, timestamp, json } = format;

const userLogger = createLogger({
   defaultMeta: { component: 'user-service' },
   format: combine(
       timestamp({
           format: 'YYYY-MM-DD HH:mm:ss'
       }),
       json()
     ),

   transports: [
       new transports.Console(),
       new transports.File({ filename: 'combined.log' })
     ]
 });
```

您还可以通过指定一个`format`参数来定制时间戳的外观，如上所示。生成的日志将类似于以下内容:

```
{"component":"user-service","level":"info","message":"Session connected","sessionID":"ak6xayY_UENoqJqXAAAA","timestamp":"2019-07-29 21:13:11"}
```

保持日志记录，以便更好地调试和维护您的项目。