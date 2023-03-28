# 不改变本地代码或 Repo 的角度定制环境。

> 原文：<https://medium.com/geekculture/angular-custom-environments-without-changing-the-local-code-or-repo-effa69457edb?source=collection_archive---------1----------------------->

![](img/83fe65decb9ed12374e6f0560718e23c.png)

The more frustrating problem you face in development, the more amazing solution you end up with.

在这篇文章中，我们将讨论一个场景，当我们想通过既不改变本地代码也不推送到存储库来设置我们的定制环境。

# **问题**

如你所知，Angular 为我们提供了存储配置的环境文件。但是我们仍然通过静态写入我们的密钥或设置来填充这些文件。如果出于安全考虑，我们不想将我们的秘密密钥共享给存储库，该怎么办？或者，如果我们想要环境中的自定义设置，但又不想推动这些更改，该怎么办？我想到的第一个理想的解决方案是建立一个环境文件，它从系统环境变量(。env)如下图。

```
function getApiBasePath(): string {
  return  (window as any).config.API_BASE_PATH ||  ${process.env.DEFAULT_API_URL}';
}export const environment = {
 production: false,
 API_BASE_PATH: getApiBasePath(),
 MSAL: {
  CLIENT_ID: ${process.env.MSAL_CLIENT_ID}',
  AUTHORITY: ${process.env.MSAL_AUTHORITY}'',
},
 debugStream: false,
};
```

不幸的是，上述解决方案根本行不通。因为我们通过`process.env`访问系统环境变量，但是`process.env`对`Node`应用程序可用，而 Angular 应用程序不可用。

# **解决方案** ✍

使用`dotenv`可以解决读取系统环境变量解的问题

1.  安装`dotenv`

```
npm i dotenv --save-dev
```

> *Dotenv 是一个零依赖模块，将环境变量从* `*.env*` *文件加载到* `[*process.env*](https://nodejs.org/docs/latest/api/process.html#process_process_env)`

2.在 Angular 应用程序的根目录下创建新文件(set-env.ts)。

这将让我们处理系统环境变量(。env)，新建或替换名为 environment . custom . ts(*target path*property)的文件。

```
const { writeFile } = require('fs');// Your environment.custom.ts file. Will be ignored by git.const targetPath = './src/environments/environment.custom.ts';// Load dotenv to work with process.envrequire('dotenv').config();// environment.ts file structureconst envConfigFile = `
  function getApiBasePath(): string {
    return (window as any).config.API_BASE_PATH  || 'default-url';
}export const environment = {
 production: false,
 API_BASE_PATH: getApiBasePath(),
 MSAL: {
  CLIENT_ID: '${process.env.MSAL_CLIENT_ID}',
  AUTHORITY: '${process.env.MSAL_AUTHORITY}',
},
 debugStream: '${process.env.DEBUG_STREAM}',
};
`;writeFile(targetPath, envConfigFile, function (err) {
 if (err) {
  throw console.error(err);
 } else {
  console.log('Using custom environment');
}
});
```

> 不要忘记设置您的环境变量。例如，在上面的例子中，在 Windows 中设置 MSAL 客户端 ID，我们运行`*set MSAL_CLIENT_ID="your-id"*`。在 MacOS 或 Linux 中可以不同。

3.将 environment.custom.ts 文件添加到`*.gitignore*` 中，以排除我们生成的 environment.custom.ts 被提交到 repo。

```
# custom files
/src/environments/environment.custom.ts
```

4.默认情况下，Angular 为我们提供了两个环境(environment.ts 和 environment.prod.ts)。现在我们应该让 Angular 知道我们有了新的定制环境文件和处理它的方法(用于服务、构建、测试)。将这个加粗的**自定义**条目添加到 angular.json for building(在 architect/build/configurations 对象内部)

```
"your-projectName": {
  ...
  "architect": {
    "build": {
      ...
      "configurations": {
        "production": {
          "fileReplacements": [
            {
              "replace": "src/environments/environment.ts",
              "with": "src/environments/environment.prod.ts"
            }
          ]
        },
      **  "custom": {
          "fileReplacements": [
            {
              "replace": "src/environments/environment.ts",
              "with": "src/environments/environment.custom.ts"
            }
          ]
        },**
      }
    ...
    }
    ...
  }
  ...
}
```

5.为 ng serve 添加自定义条目(在 architect/serve/configurations 对象内)

```
"serve": {
   "builder": "@angular-devkit/build-angular:dev-server",
   "options": {
     "browserTarget": "your-project-name:build",
     "proxyConfig": "proxy.conf.json"
},
"configurations": {
  "production": {
    "browserTarget": "your-project-name:build:production"
},
  **"custom": {
    "browserTarget": "your-project-name:build:custom"
  }
 }**
},
```

6.向 e2e 添加自定义条目(在建筑师/e2e/配置对象内)

```
"e2e": {
"builder": "@angular-devkit/build-angular:protractor",
    "options": {
      "protractorConfig": "e2e/protractor.conf.js"
      "devServerTarget": "your-project-name:serve"
},
"configurations": {
  "production": {
    "devServerTarget": "your-project-name:serve:production"
},
  **"custom": {
    "devServerTarget": "your-project-name:serve:custom"
 }
 }**
}
```

7.向 package.json 添加或修改我们的代码片段，以处理我们的 set-env.ts 文件。

```
{
   ...
   "scripts": {
      "ng": "ng",
      "config": "ts-node set-env.ts",
      "start": "npm run config && ng serve --configuration=custom",
      "build": "npm run config && ng build --configuration=custom",
      ...
   },
   ...
}
```

好了，问题解决了。让我们总结一下运行`*npm run start*`时会发生什么。

`*ts-node*`将运行`*set-env.ts*`，它将处理我们的系统环境变量并创建新文件，该文件的内容只是 set-env.ts 文件中的`*envConfigFile*`常量。最后，它将运行 ng serve，该服务器将使用我们新的 environment.custom.ts 文件作为配置。同样重要的是，我们的承诺是干净的，因为我们将我们的环境和习惯排除在变更之外。

更多信息:参考资料 [1](/@ferie/how-to-pass-environment-variables-at-building-time-in-an-angular-application-using-env-files-4ae1a80383c) 、 [2](https://dev.to/mikgross/set-up-multiple-environments-in-angular-376c) 。

就这些，希望你喜欢阅读并从中受益。☕

*关注我的* [*推特*](https://twitter.com/Vugar005)