# Nodejs 的 Prisma ORM 入门

> 原文：<https://medium.com/geekculture/getting-started-with-prisma-orm-for-nodejs-a2be625ea13e?source=collection_archive---------4----------------------->

![](img/2399088228ce9766fa495d0c68baf169.png)

当你第一次了解科技领域的许多东西时，它们听起来比实际情况更吓人。ORM(对象关系映射器)只是其中之一；对于任何 Nodejs 应用程序，我们也有相当多的选项，但我将分享一些入门 Prisma ORM 的灵感。

# 什么是 ORM？

对象关系映射的概念是，能够使用您喜欢的编程语言的面向对象范例编写如下查询以及更复杂的查询。本质上，ORM 有助于抽象向数据库编写定制 SQL 查询的复杂性。

```
SELECT * FROM user where countryId = 2
```

嗯，就像任何其他工具或库一样，我们使用它们有好处也有坏处。在很大程度上，我偶然发现了 Prisma，它可以说是中低复杂度和企业应用程序的优秀工具。

*   您可以用最少的代码快速地为 Nodejs 服务器搭建一个数据库。
*   它是开源的，与许多 SQL 兼容，如 PostgreSQL、MySQL 和 SQLite。
*   对 typescript 的现成支持。
*   许多工具都没有适当的文档，当遇到困难时我们不得不依靠社区的帮助，但是 [Prisma](https://www.prisma.io/docs/) 文档非常令人愉快并且非常详细。

接下来我们将看看本教程的一些先决条件，包括 Prisma、Express.js + typescript 和 PostgreSQL。

# 先决条件

本教程假设您对 Nodejs 有一些经验；

*   您已经安装了 Nodejs LTS
*   搭建 Expressjs 应用程序
*   Javascript/typescript 的基础知识
*   关于 ORM 如何与数据库交互的一些想法

# 履行

## 让我们设置 Express.js 应用程序

*   我们将首先为电影列表 API 演示项目设置 Express.js 应用程序。首先导航到一个新文件夹，然后运行以下命令创建一个新项目:

我们还假设您使用的是 Unix shell/终端

```
mkdir movies-listing-api
cd movies-listing-api
npm init -y
```

*   接下来，安装 Express.js，这是我们为星球大战 REST API 设计的极简框架:

```
npm install --save express
```

*   接下来，设置 typescript 开发依赖项

```
npm install typescript --save-dev
npm install [@types/node](http://twitter.com/types/node) --save-dev
```

*   接下来，创建一个 tsconfig.json 文件，tsconfig.json 是我们定义 TypeScript 编译器选项的地方。我们可以创建一个带有几个选项集的 tsconfig。它应该在项目的根目录下。

```
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```

此时，您应该有一个如下所示的 tsconfig.json:

```
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    "lib": ["es6"],                     /* Specify library files to be included in the compilation. */
    "allowJs": true,                          /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "build",                          /* Redirect output structure to the directory. */
    "rootDir": "src",                         /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). *//* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    "noImplicitAny": true,                    /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. *//* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. *//* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. *//* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. *//* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. *//* Advanced Options */
    "resolveJsonModule": true                 /* Include modules imported with '.json' extension */
  }
}
```

*   接下来，通过签入 project/src 目录来设置应用程序的入口点，并创建两个文件 app.ts 和 server.ts。

```
mkdir &&  cd src
touch server.ts
touch app.ts
```

这个文件包含了 express routing 和 handle API 请求，应该是这样的:

```
#app.ts
import express, { Application } from 'express';// express app
const app: Application = express();export default app;
```

这个 Nodejs 服务器服务于 express 应用程序，并通过 process.env.PORT 或 8000 公开它。

```
#server.ts
import app from './app';// constants
const PORT = process.env.PORT || 8000;// listen
app.listen(PORT, () => console.log(`⚡ on [http://localhost:${PORT}/api`](http://localhost:${PORT}/api`)));
```

*   接下来，我们将建立一个 PostgreSQL 数据库来为星球大战电影 API 创建表。

# 用 docker 建立本地 PostgreSQL 数据库

在项目目录的根目录下创建一个 Postgres 映像，构建该映像并运行它。

```
touch postgres.yaml
```

docker-compose 文件应该是这样的:

```
version: '3'
services:postgres:
    # For more details on configuring the Postgres Docker image, see:
    #   [https://hub.docker.com/_/postgres/](https://hub.docker.com/_/postgres/)
    image: postgres:10.3-alpine #light weight# Expose the default Postgres port on localhost
    ports:
      - '5432:5432'
      # allow connections from the host machine
      # an instance of the postgres image = postgres_docker(container)
    network_mode: bridge # port available on localhost 
    container_name: postgres_dockerenvironment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'pgpass'
      POSTGRES_DB: 'postgres'# Copy files from dbinit into the image so that they will be run on boot
    # mount db
    volumes:
      - ./initdb:/docker-entrypoint-initdb.d
```

如果您想用 Docker 运行 PostgreSQL 数据库，请使用以下命令:

```
docker compose -f postgres.yaml up
```

docker run 命令创建一个名为 postgres_docker 的新 docker 容器，将容器端口 5432 暴露给本地端口 5432。

我们的数据库已经运行了。接下来，我们将把 Prisma 添加到 Node.js 项目中，并创建我们的模式。

# 将 Prisma ORM 安装到 Node.js 项目中

运行以下命令，将 Prisma 安装为 dev 依赖项

```
npm install prisma --save-dev
```

之后，使用以下命令初始化 Prisma 模式:

```
npx prisma init
```

接下来，将数据库连接字符串添加到 env 文件中，它应该是这样的:

```
#.env
DATABASE_URL="postgresql://postgres:pgpass@localhost:5432/movies_storage?schema=public"
```

# 添加模型并运行 Prisma 迁移

根目录下的 prisma 文件夹包含 prisma.schema 文件，在其中我们定义了电影和用户的数据库表以及它们之间的关系。

```
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}generator client {
  provider = "prisma-client-js"
}model Movie {
  id        Int      [@id](http://twitter.com/id) [@default](http://twitter.com/default)(autoincrement())
  title   String [@unique](http://twitter.com/unique)
  description     StringcreatedAt DateTime [@default](http://twitter.com/default)(now())
}
```

我们已经向模式中添加了一个表。Movies 表，它有一个自动递增的 ID、必须唯一的 title 字段、description 和默认为当前日期的 createdAt 字段。

要将这些模型转换为 PostgreSQL 数据库表，请运行以下命令:

```
prisma migrate dev --name init --preview-feature
```

这将生成迁移文件，其中包含在 PostgreSQL 数据库中创建表的 SQL。

# 与电影列表交互的 API

我们将更新在上一步中创建的 app.ts 文件，并添加 POST Movies 和 GET Movies API。

```
const app: Application = express();import { PrismaClient } from '[@prisma/client](http://twitter.com/prisma/client)';
const prisma = new PrismaClient();app.get('/', (req, res) => {
  res.json({message: 'alive'});
});app.post('/movies', async (req, res) => {
  const {title, description} from 'req.body';
await prisma.movie.create({
     data: {
     title,
     description,
}
    });
   res.json({
    message: "movie created"
  });
});app.get('/movies:id', async (req, res) => {
const movie =  await prisma.movie.findUnique({
   where:{
   id: req.params.id
},
  });res.json({
    data: movie,
  });
}app.get('/movies', async (req, res) => {const allMovies =  await prisma.movie.findMany({});res.json({
    data: allMovies,
  });
});export default app;
```

# 总结

我们用 Express.js+typescript 和 Prisma ORM 构建了一个电影列表 API，使用的是运行在 Docker 上的 PostgreSQL 数据库。我知道我们仅仅触及了 Prisma 所提供的表面，尽管如此，我希望这能给你一些关于将 Prisma ORM 添加到你的 Nodejs 应用程序的见解。

最后，我建议你进一步探索一些伟大的 Prisma ORM 特性。例如，查询优化、数据库事务和聚合查询都是不错的选择。

感谢观众，希望这篇文章对你有所帮助🤗。你可以随时在 Github、T2、推特和 T4 的 LinkedIn 上联系我。一定要点赞、评论和分享😌。

编码快乐！

*最初发布于*[*https://blog . next Webb . tech*](https://blog.nextwebb.tech/getting-started-with-prisma-orm-for-nodejs)*。*