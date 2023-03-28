# NestJS 的 TypeORM 和 Mysql 配置—第 2 部分

> 原文：<https://medium.com/geekculture/typeorm-and-mysql-configuration-for-nestjs-part-2-30a288054330?source=collection_archive---------1----------------------->

![](img/c0d5b582a0568a6abb49aaf3de1a5905.png)

Photo by [Jefferson Santos](https://unsplash.com/@jefflssantos?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在上一篇文章的[中，我描述了如何使用 TypeORM 配置 NestJS 应用程序来与 Mysql 一起工作。文章中提到的方法足以构建一个快速原型，或者在学习 NestJS 框架的过程中。但是这种方法有一些明显的缺点。在我解释提到的方法的缺点之前，让我们做一些编码。](https://neilmalgaonkar.medium.com/typeorm-and-mysql-configuration-for-nestjs-1d368b42a15f)

在上一篇文章中，我们确保我们的代码接受来自`.env`文件的数据库配置，并成功连接到数据库，而不是其他。让我们改变这一点。

我们将从定义`User`实体开始。

```
**src/user.entity.ts**import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';@Entity('users') // this is table name
export class User {
  @PrimaryGeneratedColumn()
  id: number;@Column()
  firstName: string;@Column()
  lastName: string;@Column({ default: true })
  isActive: boolean;
}
```

让我们键入一个配置。我用斜体粗体突出显示了这些变化

```
**app.module.ts**import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { TypeOrmModule } from '@nestjs/typeorm';
***import { User } from './user.entity';***@Module({
  imports: [
    ConfigModule.forRoot(),
    TypeOrmModule.forRootAsync({
      useFactory: () => ({
        type: 'mysql',
        host: process.env.DB_HOST,
        port: parseInt(process.env.DB_PORT) || 3306,
        username: process.env.DB_USER,
        password: process.env.DB_PASSWORD,
        database: process.env.DB_NAME,
        entities: [
          ***User***
        ],
        synchronize: ***true***
      })
    })
  ],
  controllers: [AppController],
  providers: [AppService],
})export class AppModule {}
```

让我们关注一下 TypeORM 配置对象的`synchronize`属性。`synchronize`属性的作用是告诉 TypeORM 根据传递给配置对象的`entities`属性的实体类自动创建或更新数据库中的表。

我们可以通过运行`yarn run start:dev`并检查命令行中的任何错误来快速验证这一点。正如我在上面解释的那样，您还可以通过命令行或使用其他 GUI 工具连接到数据库，以验证它是否在工作。

如果从`User`实体中添加或删除列并保存代码，则在服务器重启时更新数据库表(**，这在每次保存**时发生)。如果你是一个新生，那么很有可能你会发现这个特性非常吸引人，事实也的确如此；如果您使用这种方法来学习 NestJS 或构建快速原型。但此功能在实际项目中是**的大忌。**

**让我们看看为什么这不是一个好主意**

1.  **因为数据库模式是自动更新的，所以对于项目的其他参与者来说，知道模式中的什么变化以及什么时候变化是一个挑战**
2.  **随着项目越来越老，数据库模式会经历多次迭代。使用自动模式更新，维护模式版本变得不可能。**
3.  **最重要的是运行测试用例。CD/CI 已经成为当今项目开发中最重要的部分。想象一个场景，您的任务是为 NestJs 项目编写测试用例，它使用了`synchronized`选项。根据我对 TypeORM 的理解，现在，没有办法在运行测试用例时使用`synchronized`选项来恢复数据库模式。**

**幸运的是，TypeORM 已经支持迁移，您可以从 cli 运行它。但是有一个警告。TypeORM cli 将总是在项目的根目录下寻找`typeormconfig.js`文件，或者您可以将`typeormconfig.js`文件的路径传递给 cli 命令。在我们的例子中，我们已经删除了`typeormconfig.js`文件，并将它的配置转移到了`.env`文件。那么你可能会问，“我们如何才能修复它？”。好吧，让我尽力解释一下我们如何实现它。我们不需要任何新的模块或安装任何新的软件包。我们已经安装了那个插件，即`@nestjs/config.`，让我们看看如何利用它来实现我们的目标。**

**请遵循下面提到的步骤。**

```
**// create **config** folder under **src** folder and add following files under **config** folder# **database.config.ts**const DatabaseConfig = () => ({
    type: 'mysql',
    host: process.env.DB_HOST || 'localhost',
    port: parseInt(process.env.DB_PORT) || 3306,
    database: process.env.DB_NAME || '',
    username: process.env.DB_USER || '',
    password: process.env.DB_PASSWORD || '',
    entities: [
        "dist/**/*.entity{.ts,.js}"
    ],
    synchronize: process.env.DB_SYNCHRONIZE || false,
    **migrationsTableName**: 'migrations', // this field will be used to create the table by name of **migrations**. You can name it whatever you want. But make sure to use the sensible name
    **migrations**: [
        "dist/src/migrations/*{.ts,.js}" // This is the path to the migration files created by typeorm cli. *You don't have to create* ***dist*** *folder. When you save file, compiled files will be stored in* ***dist*** *folder*
    ],
    cli: {
        **migrationsDir**: "src/migrations" // This path will be used by typeorm cli when we create a new migration
    }
});export default DatabaseConfig;-------------------------# **app.config.ts**import DatabaseConfig from './database.config';export default () => ({
    environment: (process.env.NODE_ENVIRONMENT) ? process.env.NODE_ENVIRONMENT : 'development' ,
    port: 3000,
    database: {
        ...DatabaseConfig()
    }
});**
```

**现在，我们已经完成了应用程序配置的定义。现在让我们看看如何使用应用程序配置。我们将从更新`app.module.ts`开始**

```
****app.module.ts**import { Module } from '@nestjs/common';
import { ConfigModule, **ConfigService // 1.** } from '@nestjs/config';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { TypeOrmModule } from '@nestjs/typeorm';
**import { ConnectionOptions } from 'typeorm';**
**import AppConfig from './../config/app.config';**[@Module](http://twitter.com/Module)({
  imports: [
    **ConfigModule.forRoot( // 2.
        imports: [** [**isGlobal**](https://docs.nestjs.com/techniques/configuration#use-module-globally)**: false,
            load: [
                AppConfig // 3.
            ]
        ],
    ),**
 **TypeOrmModule.forRootAsync({ // 4.
        imports: [
            ConfigModule
        ],
        useFactory: (configService: ConfigService) => { // 3.
            return configService.get<ConnectionOptions>('database');
        },
        inject: [
            ConfigService
        ]
    }),**
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}**
```

**让我们检查一下`app.config.ts`和`database.config.ts`文件。在我开始之前，让我明确一点，`app.config.ts`文件并不是真正必要的。我添加了这个文件来创建一个可以在整个项目中使用的中央配置对象。现在让我们检查数据库配置文件。请仔细验证数据库配置结构，并将其与`typeormconfig.js`文件中定义的对象进行比较。这是故意的。我将在本文的后面回到这个问题。**

**现在，我们将继续讨论`app.module.ts`文件。我在上面的代码中添加了编号的点，下面我将尝试逐一解释**

1.  **`ConfigService`是由 NestJS 的配置模块公开的服务。我们将使用这个服务来访问环境变量。例如`configService.get('ENV_VARIABLE_NAME')`**
2.  **在前面的例子中，我们使用了默认的配置模块调用，它自动从项目的根目录加载`.env`文件。在这里，我们用一个稍微不同的版本替换了它，它做了更多的事情。`isGlobal`属性使`ConfigModule` [成为全局](https://docs.nestjs.com/techniques/configuration#use-module-globally)**
3.  **属性是一个有趣的属性。它所做的是获取一个函数数组，一旦加载了`ConfigModule`就会调用这个函数。例如，看一下`app.config.ts`文件**
4.  **这与我们的第一种方法非常相似。但是在这里，不是直接访问，`env`我们将使用由`ConfigModule.`导出的`ConfigService`，在我们继续之前，我们需要确保`ConfigModule`正在被导入到`TypeORM.forRootAsync`调用中，并且我们**需要**注入`ConfigService`以使其在`useFactory`函数中可用。**

**到目前为止，我们所做的更改只是让之前的实现变得更加优雅。此外，我们已经为从命令行运行迁移奠定了基础。**

**让我们在`config`文件夹中创建一个名为`typeorm.config.ts`的新文件**

```
**# **typeorm.config.ts**import { ConnectionOptions } from 'typeorm';
import DbConfig from './database.config';
import * as dotenv from 'dotenv';**dotenv.config(); // very very important!!**const typeormConfig = DbConfig() as ConnectionOptions;export default typeormConfig;**
```

**我们上面所做的是，导入 typeorm 所需的数据库配置。如果你一直关注到现在，你会意识到我们还没有在 NestJS 的任何模块中导入`typeorm.config.ts`文件。这是因为我们将使用这个文件作为 TypeORM 的 cli 工具的配置。**

**在上面的代码中需要注意的另一件重要的事情是，我们已经显式地导入了`dotenv`包并调用了`dotenv.config()`函数，该函数从项目的根解析`.env`文件，并使`.env`变量在代码执行上下文中可用。我们必须这样做，因为当我们从 cli 运行迁移时，我们不是在 NestJS 的生态系统中运行它们，而是访问常规的`.ts`文件，它不知道 **NestJS 的存在**。**

**现在，最后一步是更新`package.json`文件。我在下面的代码片段中添加了成功运行迁移所需的东西。**

```
**{
  ....
  "scripts": {
    ....
    **"typeorm": "node --require ts-node/register ./node_modules/typeorm/cli.js",** // Shortcut for typeorm cli **"create-migration": "yarn run typeorm migration:create --config src/config/typeorm.config.ts -n",** // this command is used for creating migrations **"run-migrations": "yarn run typeorm migration:run --config src/config/typeorm.config.ts",** // this command runs the created migrations **"revert-migrations": "yarn run typeorm migration:revert --config src/config/typeorm.config.ts ",** // this is used for reverting the migrations **"generate-migrations": "yarn run typeorm migration:generate --config src/config/typeorm.config.ts "** // This command can be used to generate migrations by difference betweeb entity and existing database table structure
  },
  ....
}**
```

**如果您仔细检查代码片段，您会注意到大多数命令都带有`--config`参数，并且它指向在`config`文件夹中创建的`typeorm.config.ts`。**

**如果您想了解更多关于迁移的信息，请访问 TypeORM 的迁移部分[这里](https://github.com/typeorm/typeorm/blob/master/docs/migrations.md)。**

**我已经在 Github 上上传了完整的代码，你可以在这里找到[。我在 Github 存储库上构建代码的方式略有不同，但核心概念仍然是一样的。](https://github.com/neilmalgaonkar/nestjs-learning/tree/nestjs-db-config-service)**