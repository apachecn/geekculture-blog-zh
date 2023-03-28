# 消除服务中的重复:Swagger/OpenAPI 和 AJV

> 原文：<https://medium.com/geekculture/de-duping-the-duplication-in-services-featuring-swagger-openapi-and-ajv-abd22c8c764e?source=collection_archive---------10----------------------->

![](img/f289c93b1540d84c98c643f9f0c6cf08.png)

好的设计更容易改变。然而，当涉及到 API 文档和服务验证时，ETC 的这一原则往往会被忽略。在这里，不要重复你自己[干]的租户经常被忽略，留下了具有多个文件的服务，这些文件可能跨越数百甚至数千行代码，具有大量的重复。

带有整体验证引擎和华而不实的文档的服务开发变成了一种技术债务。由于这些引擎和文档经常位于正在改变的代码表面区域之外，它们变得不同步的可能性增加了。

# **那么解决办法是什么？**

我提出了一种新的设计模式来开发您的 swagger 文档，然后让您的 OpenAPI 规范驱动您的验证。

有了上面的使命陈述，让我们确保我们都在工具链的同一页上。NodeJS 和 JavaScript 生态系统是什么，这是理解我们最终目标的重要一步。

```
**Service Documentation**: Swagger 3.0 -- OpenAPI**Service Validation Engine**: AJV**node-modules**: swagger-jsdoc, openapi-validator-middleware**NodeJS Framework**: Express
```

虽然我承认存在其他验证引擎(JOI 和 express-validator，仅举几个例子),但 AJV 提供了一个简单的 JSON feed，而且人们已经为它编写了 OpenAPI 包装器！至于 NodeJS 框架，我选择使用 express，因为那是我更熟悉的。没有理由不支持 koa，因为这个包甚至支持 koa！

# 那么，您具体是如何消除重复的呢？

以上每个包都有一个具体的目标。

对于`swagger-jsdoc`,我们将坚持早先的说法，即更容易改变。我们将把我们的 swagger 定义放在路由文件中。这将允许未来的开发人员看到与代码共存的规范，使他们更清楚地知道，当他们在途中更改代码时，要更改那个规范。

`openapi-validator-middleware`能够消费生成的 OpenAPI Swagger 文档，并将其用于验证引擎。这个包是 AJV 的一个包装器，它允许我们对大量的重复删除进行最小的代码修改。

# 那么这看起来像什么？

因此，让我们从验证部分开始，为此，我们看一下描述我们的 express 应用程序的文件`app.js`。

那就先做最重要的事；让我们导入我们的模块

```
const swaggerValidation = require(‘openapi-validator-middleware’);
```

在导入之后，我们只需要将它指向我们的 Swagger 文档来配置它。

```
swaggerValidation.init('swagger.yml');
```

有了用我们的 swagger 配置的验证引擎，我们只需要在我们的路由定义中将它作为中间件来实施。

```
api.get('/simple', swaggerValidation.validate, getSimple)
```

有了这 3 行代码，我们已经配置了我们的验证引擎，根据我们的 swagger 规范对它进行了调整，现在它正在针对`/simple`路线执行它的规则。你不再需要维护一个单独的文件 Joi/AJV 文件来维护你的服务验证——酷吧？

# 好吧，但是关于斯瓦格的文件？那不是很可怕吗？

答案是肯定的；因为您的 swagger 文件现在必须包含所有的验证逻辑，所以它会很大——但是它应该已经包含了这些信息。考虑到这一点，我们将让我们的另一个包`swagger-jsdoc`来负责维护 swagger 文件。我们的目标更容易改变，记得吗？因此，我们将把我们的 swagger 定义与我们的路由文件逻辑放在一起。由于代码和文档都放在一个地方，当开发人员进行更改时，他们有望更积极地保持一切同步。更不用说任何改变参数/请求体的验证需求的需求也会立即反映在 swagger 文档中。

这是我们之前定义的`get-simple.js`

```
/**
 * [@openapi](http://twitter.com/openapi)
 *  /v1/acme:
 *    get:
 *      description: a simple get route that returns the `foo` query param
 *      parameters:
 *        - in: query
 *          name: foo
 *          schema:
 *            type: string
 *            minimum: 3
 *      responses:
 *        200:
 *          description: a object witth the echoed query param.
 *          content:
 *            type: object
 *            properties:
 *              foo:
 *                type: string
 *                minimum: 3
 */
const getSimple = (req, res) => {
  const { foo } = req.query;return res.status(200).json({ foo });
};module.exports = getSimple;
```

# 等等，我有一些问题！

> 一个 4 行的路由文件有 20 行注释吗？为什么 foo 是重复的？我以为我们在消除重复？

要回答这些问题，是的，你会有一大堆文档。这是不可避免的，因为我们需要有 swagger 的外壳，但它应该有助于新开发人员查看该文件，了解请求和响应的期望。

至于你看到的复制品，我马上就要讲到了！这是为了方便显示复制。利用 YAML 的特点，我们实际上可以消除一些重复，同时进一步划分我们的定义。

# 好的——开始吧，你是怎么做的？

利用 YAML 锚，我们可以为我们的字段创建类似变量的原子定义。但是首先，让我们把我们的服务做得更好一些，创建一些文件/目录。

```
mkdir swagger
touch swagger/first-name.yml
touch swagger/last-name.yml
touch swagger/user-id.yml
```

如您所见，这个 swagger 文件夹将包含我们所有的 swagger 组件定义。这将确保我们的定义在各种途径中使用时保持一致，同时消除重复，因为它们现在可以共享一个真实的来源——这个文件夹。

**档案**

```
*# swagger/first-name.yml*
x-template:
  firstName: &firstName
    type: string
    minimum: 1
    maximum: 30
    description: the first name of our acme user# swagger/last-name.yml
x-template:
  lastName: &lastName
    type: string
    minimum: 1
    maximum: 30
    description: the last name of our acme user# swagger/user-id.yml
x-template:
  userId: &userId
    type: string
    minimum: 4
    maximum: 4
    pattern: '[0-9]{4}'
    description: the unique identifier of our acme user
```

随着我们的大摇大摆领域的组成部分创建，让我们自旋一些新的路线使用我们的新领域！

***put-create.js***

```
/**
 * [@openapi](http://twitter.com/openapi)
 *  /v1/acme/create:
 *    put:
 *      description: creates a fake user of the acme service
 *      requestBody:
 *        content:
 *          application/json:
 *            schema:
 *              type: object
 *              required:
 *                - firstName
 *                - lastName
 *              properties:
 *                firstName: *firstName
 *                lastName: *lastName
 *      responses:
 *        200:
 *          description: a object with the echoed firstName, lastName and a random userId.
 *          content:
 *            type: object
 *            properties:
 *              firstName: *firstName
 *              lastName: *lastName
 *              userId: *userId
 */
const putCreate = (req, res) => {
  const { firstName, lastName } = req.body;
  const userId = Math.floor(1000 + Math.random() * 9000);return res.status(200).json({ firstName, lastName, userId: `${userId}` });
};module.exports = putCreate;
```

看，我们已经创建了一个更复杂的请求/响应对象，注释的总行数增加了 3 行！最重要的是，即使你对这个文件没有经验，你也可以通过阅读第一个注释来确定它的用例以及请求/响应契约。看到更容易改变的福利了吗？假设您有允许 60 个字符姓氏的需求，您可以简单地更改 swagger 文件`last-name.yml`,您将得到更新的 Swagger 文档以及执行它的验证规则！

# 好吧——我被说服了，但是你如何把这些评论变成一个狂妄自大的医生呢？

*swagger-generator.mjs*

```
import fs from 'fs';
import swaggerJsdoc from 'swagger-jsdoc';
import { dirname } from 'path';
import { fileURLToPath } from 'url';
import packageJson from './package.json';const __dirname = dirname(fileURLToPath(import.meta.url));const options = {
  format: '.yml',
  definition: {
    openapi: '3.0.0',
    info: {
      title: packageJson.name,
      version: packageJson.version,
    },
  },
  apis: ['./src/routes/*.js', './swagger/**/**.yml'], // files containing annotations
};const runtime = async () => {
  try {
    const openapiSpecification = await swaggerJsdoc(options);
    fs.writeFileSync(`${__dirname}/swagger.yml`, openapiSpecification);
  } catch (e) {
    console.log('broke', e);
  }
};runtime();
```

上面的脚本将生成 OpenAPI 规范，并生成验证引擎将使用的`swagger.yml` 。为了帮助实施良好的实践，并且因为所有的开发人员(包括我自己)都不善于记住事情，所以我个人利用 Husky 来确保生成这个文件。这将作为一个预提交钩子来完成，它将运行上面的脚本，后跟一个`git add swagger.yml`命令。

# 但是你怎么能强制执行呢？

CI CI CI！因为我们只有一个预提交钩子来生成我们的 swagger.yml，所以有一个合理的担心。毕竟，比没有文档更糟糕的是糟糕的/过时的文档。

> 如果开发人员用`-n`提交或者只是从 web-ui 提交，会怎么样

好吧，让我先说他们是一个怪物(特别是如果他们和`-n`在一起的话)！).但是为了帮助实施这一点，在创建/捆绑您的应用程序时，这应该是一个构建步骤。对于测试用例，我们可以重新运行`swaggerJsDoc`命令，并直接将其输出与`swagger.yml`输出进行比较。任何差异并停止执行。

# 示例/参考

## 回购展示了这一过程:

[](https://github.com/goldsziggy/ms-acme-openapi-ajv) [## goldsziggy/ms-acme-openapi-ajv

### 本报告的目的是为媒体文章提供帮助。本回购中的代码不代表…

github.com](https://github.com/goldsziggy/ms-acme-openapi-ajv) 

## swagger-jsdoc

[](https://www.npmjs.com/package/swagger-jsdoc) [## swagger-jsdoc

### 这个库读取 JSDoc 注释的源代码，并生成一个 OpenAPI (Swagger)规范。想象一下拥有…

www.npmjs.com](https://www.npmjs.com/package/swagger-jsdoc) 

## open API-验证器-中间件

[](https://www.npmjs.com/package/openapi-validator-middleware) [## open API-验证器-中间件

### 该软件包根据 Swagger/OpenAPI 定义在 Express、Koa 或 Fastify 应用程序中提供数据验证…

www.npmjs.com](https://www.npmjs.com/package/openapi-validator-middleware)