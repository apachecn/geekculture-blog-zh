# 5 月 3 日:用 Express 和 Sequelize 用 JavaScript 构建 PostgreSQL API(第 1 部分)

> 原文：<https://medium.com/geekculture/may-4-building-a-postgresql-api-in-javascript-with-express-and-sequelize-part-1-b0da9a3e1583?source=collection_archive---------12----------------------->

![](img/b9709b4c5bb353c1e60eec0f57250355.png)

© 2003 Universal Pictures/Buena Vista Entertainment

在 Flatiron 学校期间，我学习了 web APIs、数据库和对象关系映射器(ORM)的基础知识——它们协同工作，赋予 web 应用程序难以置信的功能，帮助我们塑造我们的世界。我们在 [**Ruby on Rails**](https://rubyonrails.org/) 中学到了这一切，这是一个很棒的框架，易于使用，相对安全，并由一个充满活力的社区和[大量免费宝石](https://rubygems.org/)支持。

可悲的是，尽管许多产品仍然依赖 Rails，[它的使用似乎正在下降](https://sloboda-studio.com/blog/is-ruby-on-rails-dying/)。在许多情况下，Rails 可能会僵化、不灵活且运行缓慢，尽管最近的版本已经添加了一些特性来解决这些问题。然而，就业市场似乎正在向 JavaScript、Python 和 PHP 发展。

考虑到这一点，并本着不断改进和教育的精神，我决定在从 Flatiron 毕业后扩展并学习其他 web 应用程序框架……但是当然没有大量适合初学者的资源。所以接下来是我的 JavaScript web API 食谱**的尝试，目标是那些从 Rails 过渡的人。**

**这是一个很大的话题，**而且我毫不怀疑这个教程不会公正地对待这个话题。事实上，内容太多了，我不得不将本教程分成几个部分；这第一部分将引导你设置你的项目，配置和环境。

# 背景

请注意，本教程假设如下:

*   您熟悉 JavaScript 语法和 bash 命令；和
*   您有一个正在运行的 [PostgreSQL](https://www.postgresql.org/) 安装。下载一个安装程序并阅读安装说明[这里](https://www.postgresql.org/download/)。

Sequelize 可以与许多 SQL 方言一起工作，包括 MySQL、MariaDB、SQLite3 和 Microsoft SQL Server。为了简洁起见，我将在本教程中使用 PostgreSQL，因为它与我使用的云服务配合得很好，也是我最熟悉的。**请评论**如果你对使用 Sequelize 和另一种风格的 SQL 的教程感兴趣，我可能会写它。

# 1:在新目录中初始化 NPM 并安装软件包

从终端开始，为你的 API 创建一个目录并进入其中。用`yarn init --yes`初始化一个新的节点项目，你的目录里会出现一个百搭的`package.json`。`--yes`标志只是自动生成`package.json`；随意省略它，但我发现它更容易，因为你可以随时手动编辑它。

如果你用的是 Github(强烈推荐！)，你的遥控器将在每次推送中包含依赖项，如果它们没有在你的初始提交中`.gitignore` d。这是多余的，会减慢速度，所以现在是在项目目录的根目录下创建一个`.gitignore`文件的好时机，只需下面两行:

```
node_modules/
.env
```

在创建了一个`.gitignore`文件之后，用`git init && git add . && git commit -m "initial commit" && git push`在终端中初始化并推送您的初始提交。

最后，用`yarn add bcryptjs cors dotenv express jsonwebtoken pg pg-hstore sequelize --save && yarn add nodemon sequelize-cli --dev`在终端中安装您需要的依赖项。这个令人不安的长终端命令将安装下面列出的依赖项，按字母顺序(*而不是*重要性排序！):

## 生产依赖性

*   `**bcryptjs**` **:** 用于哈希敏感数据，尤其是密码
*   `**cors**` **:** 允许跨产地资源共享(CORS)，确保您的前端和后端安全、正确地共享数据
*   `**dotenv**` **:** 用于协调和加载环境变量——并非绝对必要，但非常有用
*   `**express**` **:** 我们的应用框架—点击阅读更多关于 Express [的信息](http://expressjs.com/)
*   `**jsonwebtoken**` **:** 用于实现 [JSON web 令牌](https://tools.ietf.org/html/rfc7519)，它将我们发送的数据标准化
*   `**pg**` **和** `**pg-hstore**` **:** 用于连接到 PostgreSQL 服务器并序列化我们从中存储/检索的数据
*   `**sequelize**` **:** 一个对象关系映射器，就像 Ruby on Rails 中的 ActiveRecord 在这里阅读更多关于 Sequelize [的内容](https://sequelize.org/master/)

## 开发依赖性

*   `**nodemon**` **:** 支持“热加载”，因此我们可以立即看到更改，而不必关闭并重启我们的服务器
*   `**sequelize-cli**` **:** 用于创建顺序化项目和定义顺序化对象/关系

运行上面的命令后，您应该会看到一个`node_modules`文件夹出现，您的`package.json`文件应该如下所示:

```
{
   "name": "My Express API",
   "version": "1.0.0",
   "description": "A simple Express API",
   "main": "index.js",
   "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
   },
   "keywords": [],
   "author": "Josh Frank",
   "license": "ISC",
   "dependencies": {
      "bcryptjs": "^2.4.3",
      "cors": "^2.8.5",
      "dotenv": "^8.2.0",
      "express": "^4.17.1",
      "jsonwebtoken": "^8.5.1",
      "pg": "^8.6.0",
      "pg-hstore": "^2.3.3",
      "sequelize": "^6.6.2"
   },
      "devDependencies": {
      "nodemon": "^2.0.7",
      "sequelize-cli": "^6.2.0"
   }
}
```

`add`、`commit`和`push`提交您的进度。

# 2:创建快速应用程序

在根目录中创建一个文件`index.js`，并在其中填充一些基本代码，以启动您的 Express 应用程序:

```
const express = require( "express" );
const cors = require( "cors" );
const app = express();const port = 3000;app.use( express.json() );
app.use( express.urlencoded(.{ extended: true } ) );
app.use( cors( { origin: `http://localhost:${ port }` } ) );app.get( "/", ( request, response ) => response.send( "Test" ) );app.listen( port, () => console.log( `Listening: port ${ port }` ) );
```

快速逐行分解:

1.  我们`require`了`"express"`和`"cors”`包，它们监听请求、验证请求并返回响应；
2.  我们创建一个 Express `app`对象和一个`port`号码来监听；
3.  我们告诉我们的`app`将请求数据组织和解析为`.json()`数据，忽略不正确的请求数据`.urlencoded()`，并允许来自`localhost`的请求，这样我们就可以在开发时进行测试；
4.  我们用`app.get()`定义了一个默认的`GET`路由，它以一个路径(`"/"`)和一个回调作为参数；
5.  在我们的`app.get()`函数中，我们编写了一个带有两个参数`request`和`response`的回调函数，告诉 Express 如何处理对`"/"`的`GET`请求——在本例中，回调一个字符串`"Test"`；
6.  最后，我们在我们定义的`port`上监听请求，当我们开始监听时`console.log`发送消息。

现在，当你在终端中运行`node index.js`时，你会看到一条消息`Listening on port 3000`，你会看到你的终端在应用程序监听时冻结。打开第二个终端，运行你的应用程序，运行`curl localhost:3000`到`GET`你的测试路线，你会看到`Test`返回作为响应。**成功了！你现在有一个工作的快递应用程序！**

当然，最终我们的 API 会变得更加复杂，做更多有趣的事情，而不仅仅是简单地吐出`Test`。所以我强烈推荐使用 REST 客户端来尝试路线，而不是使用浏览器或`curl`。最受欢迎的好像是[邮递员](https://www.postman.com/)；我更喜欢 ARC ，因为它是开源的。

有趣的是，在这一点上，如果你改变`index.js`，你保存的更改不会改变你的应用程序的行为，直到你关闭并重新启动应用程序。通过在第 11 行更改您的应用程序的`response`来亲自查看:

```
...
app.get( "/", ( request, response ) => response.send( "A different test" ) );
```

如果您在终端*中运行`curl localhost:3000`而不重启*`*app.js*`*，您仍然会看到`Test`的响应。反复关闭和重启我们的服务器很乏味，所以让我们使用`nodemon`来强制我们的应用程序“热加载”并立即反映保存的更改。在您的`package.json`文件中，删除自动生成的`test`脚本并替换为:*

```
*...
"scripts": {
   "dev": "nodemon index.js"
},
...*
```

***您刚刚定义了一个节点脚本**——构建应用程序时经常使用的终端命令的快捷方式。在您的终端中运行`nodemon app.js`将使用`nodemon`而不是直接使用`node`来启动您的 Express 应用程序，因此每次保存后都会重新启动。但是多亏了您的脚本，您可以通过运行`yarn dev`来做同样的事情，结果是:*

```
***yarn run v1.22.10** *$ nodemon app.js* [nodemon] 2.0.7
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node index.js`
Listening: port 3000*
```

*现在，每次保存更改时，您的应用程序都会自动刷新。随着我们构建 Express 应用程序，我们将添加更多的节点脚本。*

# *3.配置`dotenv`以提供环境变量*

*在我们继续之前，让我们了解一下`process.env`，它代表什么，以及如何使用我们安装的`dotenv`包来充分利用它。*

*每个节点项目都有一个`process.env`全局变量，代表您的应用程序所处的系统环境的状态。我们需要它，因为如果我们真的想使用它，我们需要**将应用部署**到服务器，不同的服务器代表不同的环境。我们可能会在`127.0.0.1`本地测试一个应用，然后在`51.16.0.9`将其部署到一个服务器上，但由于某种原因，在`172.0.7.0`又不得不切换到一个服务器上。有了`process.env`，我们可以在任何环境下正确连接，而不必为每个环境编写代码。*

*我们可以用几种方式*提供*(定义&使用)`process.env`变量——最简单的是在应用程序级别，代码如下:*

```
*const process.env.TEST = "Test";
console.log( process.env.TEST );*
```

*您也可以使用`export`终端命令:在您的终端中运行`export TEST="Test"`，在项目中的任何地方使用`process.env.TEST`都会产生字符串`"Test”`。*

*这两种方式都很好，但是动态配置可能会很快失控。**进入** `**dotenv**` stage right，这允许我们在一个方便的地方定义和加载我们所有的环境变量。这不仅节省了我们的脑细胞和治疗费用，而且[让我们的应用程序更快、更有用](https://12factor.net/config)——所以强烈推荐使用`dotenv`，尽管如我上面所说，这并不是绝对必要的。*

*因为我们已经安装了`dotenv`，我们所要做的就是在我们项目的根目录下创建一个`.env`文件，并向其中添加如下代码:*

```
*PORT = 3000
TEST = "dotenv test"*
```

*`.env`只是一个原始文本文件，**不是 JavaScript，**所以不用分号。*

*接下来，在`package.json`中，让我们告诉节点脚本我们刚刚定义的所有环境变量:*

```
*...
"scripts": {
   "dev": "nodemon -r dotenv/config index.js"
},
...*
```

*最后，在`index.js`的顶部，让我们`require( "dotenv" )`，删除我们的`port`变量并使用那些环境变量来代替:*

```
***require( "dotenv" ).config();**
const express = require( "express" );
const cors = require( "cors" );
const app = express();app.use( express.json() );
app.use( express.urlencoded( { extended: true } ) );
app.use( cors( { origin: `http://localhost:${ **process.env.PORT** }` } ) );app.get( "/", ( request, response ) => response.send( **process.env.TEST** ) );app.listen( **process.env.PORT**, () => console.log( `Listening: port ${ **process.env.PORT** }` ) );*
```

*保存您的更改，这样`nodemon`将重新启动您的应用程序，当您在另一个终端`curl localhost:3000`时，您将得到`dotenv test`作为响应，确认`dotenv`现在已成功配置和供应！*

*如果你正在使用 Github，现在是一个好时机去`add`、`commit`和`push`。你会记得我们在`.gitignore`文件中添加了`.env`；这是因为随着我们继续构建我们的应用程序，我们将向我们的`.env`文件添加关于我们的数据库和授权令牌的敏感信息。我们绝对不希望这些东西出现在一个公共的 Github 仓库中。*

*此时，您的应用程序的文件结构应该如下所示:*

```
*├── node_modules
│   └── *...a LOT of packages in folders*
├── .env
├── .gitignore
├── index.js
├── package.json
└── yarn.lock*
```

# *4.使用`sequelize-cli`设置序列*

*我们现在可以开始设置我们的应用程序来与 PostgreSQL 数据库交互。我们将在`sequelize-cli`的帮助下完成这项工作，这是一个命令行界面，让我们可以用简单的命令生成项目、模型和迁移。*

*首先，通过在您的项目根目录中创建一个名为`.sequelizerc`的新文件并用下面的代码填充它来配置`sequelize`:*

```
*const path = require( "path" );module.exports = {
   "config": path.resolve( "./app/config", "database.config.js" ),
   "models-path": path.resolve( "./app/models" ),
   "seeders-path": path.resolve( "./app/seeders" ),
   "migrations-path": path.resolve( "./app/migrations" )
};*
```

*这只是导出一个 JSON 对象，将 Sequelize 指向它需要的一些文件。我们还没有创建它们，但是这些文件的名字应该是不言自明的:`config`将保存数据库配置，`models`将保存我们的模型，`seeders`保存我们项目的种子(样本/测试)数据，`migrations`保存迁移。*

*现在，在根目录下创建一个名为`app`、`cd`的文件夹，放入你的终端，戴上帽子，运行`sequelize-cli init`。瞧！你的序列化项目现在有了它的`config`、`models`、`seeders`和`migrations`文件。您的应用程序的文件结构现在应该如下所示:*

```
*├── node_modules
│   └── *...package folders* ├── app
│   └── config
│   │   └── config.json
│   └── models
│   │   └── index.js
│   └── migrations
│   └── seeders
├── .env
├── .sequelizerc
├── index.js
├── package.json
└── yarn.lock*
```

*在我们继续之前，让我们绕道到我们的`.env`文件，并添加一些 Sequelize 需要与 PostgreSQL 通信的重要变量:*

```
*PORT = 3000
DEV_DATABASE_HOST = "127.0.0.1"
DEV_DATABASE_USERNAME = "postgres"
DEV_DATABASE_PASSWORD = "postgres"*
```

## ***快速提示:***

> ****配置 PostgreSQL 可能会非常令人沮丧，*** *不幸的是，这个话题超出了本教程的范围。如果你在本地机器上用* `*localhost*` *，* `*HOST*` *将会是* `*127.0.0.1*` *，你的* `*USERNAME*` *和* `*PASSWORD*` *将会是你首选的本地 PostgreSQL 用户的用户名/密码；如果您从未更改它们，它们都将是默认值* `"postgres”` *。如果您使用外部主机，您的主机公司几乎肯定会提供一套在其服务器上配置 PostgreSQL 数据库的说明，以及一个列出主机地址/用户名/密码的“设置”或“配置”面板。网上有很多有用的指导；评论，我可能会写一些。**

*待办事项列表中的下一个任务将涉及我们在 app 文件夹中自动生成的文件。我们将从`config/config.json`开始，因为我们需要它来匹配我们的`.sequelizerc`路径并使用我们的环境变量。像这样调整它:*

1.  *将文件重命名为`database.config.js`；*
2.  *在`config`对象之前添加`module.exports = { ... }`，通过移除所有键周围的引号，将其从 JSON 对象重构为 JavaScript 对象；*
3.  *将下面两行代码添加到文件顶部的`require( "dotenv" )`中，并析构那些我们刚刚从`.env`环境中创建的重要数据库变量；和*
4.  *将`database:`名称更改为您的项目名称，将 SQL `dialect:`更改为`"postgres"`。*

*完成后，`database.config.js`应该是这样的:*

```
*require( "dotenv" ).config();const { DEV_DATABASE_HOST, DEV_DATABASE_USERNAME, DEV_DATABASE_PASSWORD } = process.env;module.exports = {
   development: {
      username: "root",
      password: null,
      database: "my_express_app_development",
      host: "127.0.0.1",
      dialect: "postgres"
   },
   test: {
      username: "root",
      password: null,
      database: "my_express_app_test",
      host: "127.0.0.1",
      dialect: "postgres"
   },
   production: {
      username: "root",
      password: null,
      database: "my_express_app_production",
      host: "127.0.0.1",
      dialect: "postgres"
   }
}*
```

*最后，在我们的`development:`对象中使用那些`.env`环境变量。`test:`和`production:`在我们准备好部署之前不会发挥作用，所以现在可以随意注释掉它们并忽略它们。*

```
*...
module.exports = {
   development: {
      username: **DEV_DATABASE_USERNAME**,
      password: **DEV_DATABASE_PASSWORD**,
      database: "my_express_app_development",
      host: **DEV_DATABASE_HOST**,
      dialect: "postgres"
   },
   ...
}*
```

*接下来让我们看一下我们的另一个自动生成的文件，`models/index.js`。更改第 8 行，使我们的`config`变量指向正确的位置— `database.config.js`，而不再是`config.json`。往下看，您会看到实际使用我们的`config`来启动`new Sequelize()`的逻辑:*

```
*'use strict';const fs = require('fs');
const path = require('path');
const Sequelize = require('sequelize');
const basename = path.basename(__filename);
const env = process.env.NODE_ENV || 'development';
**const config = require( __dirname + "/../config/database.config.js" )[ env ];** const db = {};let sequelize;
if (config.use_env_variable) {
sequelize = new Sequelize(process.env[config.use_env_variable], config);
} else {
sequelize = new Sequelize(config.database, config.username, config.password, config);
}*
```

*在终端运行`yarn dev`。如果您看到错误，请回顾本教程，确保对样板`sequelize`代码的所有更改都是正确的，没有打字错误。如果一切都按计划进行，神在微笑，你应该看到一个熟悉的`Listening: port 3000`消息，没有错误。`add`、`commit`和`push`提交您的进度。*

# *下一集*

*我们的 Express 应用程序已创建，我们的环境已调配，我们的顺序化配置也已就绪。我们已经覆盖了很多领域，我们已经为本教程的第 2 部分做好了准备，在这里我们将创建模型、关联和迁移。*

*稍后，我们将简要介绍注册用户、使用`bcryptjs`散列他们的密码，以及使用`jsonwebtoken`认证他们的连接。所有这些将为您提供一个基本但重要的知识基础，以便使用 Express 构建一个健壮且专业的 JavaScript 后端！*