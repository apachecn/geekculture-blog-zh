# 使用 OpenAPI/Swagger 文档增强您的 Angular UI

> 原文：<https://medium.com/geekculture/power-your-angular-ui-with-openapi-docs-282aff5a9366?source=collection_archive---------6----------------------->

## 生成 API 模型和端点，使应用程序在构建时安全并与后端兼容。

![](img/b32c0df3579a6515049d5a423abbd57a.png)

Ref: [https://images.app.goo.gl/BR74Z8VAmjV355XCA](https://images.app.goo.gl/BR74Z8VAmjV355XCA)

您是否遇到过这样的问题:某个属性或 API 端点发生了变化，而您在应用程序部署之前不会意识到这一点？如果有一种方法可以让你的用户界面通过后端 DTO/端点安全地输入你的，这不是很方便吗？

如果我告诉你有一种方法，我们可以获取你的 API 的 swagger/OpenAPI 契约，然后自动生成模型和服务，你可以用它们来使你的 API 的 UI 类型安全。令人惊奇的是，所有这些都可以在构建时进行配置。

介绍够了，让我们开始吧！！

## **先决条件:**

1.  棱角分明(4。x)或更新的项目。
2.  Node 10.x 或更高版本。

这就是你所需要的。

## 1.第一步

首先让我们安装一个 NPM 包，它允许你读取一个配置 JSON 和你的合同来生成你的资源。

```
npm i ng-openapi-gen-cli --save-dev
or
yarn add ng-openapi-gen-cli -D
```

## 2.第二步

在项目根目录下创建一个名为 generate-types 的目录。你可以随意命名它，我建议把它从你的代码中分离出来，放在你的项目根目录中。以便在构建过程中使事情变得更容易。

***** 如果你正在使用 monorepo 结构，那么确保它在你的 UI 应用程序的根目录下。

## 3.第三步

在 generate-type 目录下创建一个 JSON 文件' **test-config.json** '。该文件将包含生成您的文件所需的所有配置。

以下是它的外观示例:

```
{
  "$schema": "../../../node_modules/ng-openapi-gen/ng-openapi-gen-schema.json",
  "input": "<uri-for-your-open-api-docs-contract>",
  "output": "apps/learn-nestjs-task-management/generate-typings/api",
  "ignoreUnusedModels": false,
  "module": "GeneratedApiModule",
  "serviceSuffix": "ApiService",
  "modelSuffix": "ApiModel",
  "indexFile": true
}
```

让我给你解释一下这个物体是做什么的

1.  **$schema:** 它引用 ng-open-api-gen 的父原理图，该原理图包含生成资源所需的所有细节。
2.  **输入:**这是我们添加 openapi/swagger 契约的 uri 的地方。它应该看起来像“<域> /api-docs”。
3.  **输出:**您想要生成文件的位置。所以在这个例子中，所有的文件都将在“generate-typing/API”目录中生成。我建议使用这种结构，因为它让你更容易在一个地方找到所有的模型。
4.  **模块:**这允许你为所有生成的资源创建一个模块，使得在你的应用或模块中导入该模块变得容易。
5.  **serviceSuffix:** 默认生成所有不带后缀的服务。建议在它后面加上后缀，以便更容易区分 UI 服务和生成的 API 服务。
6.  **型号后缀:**类似于**服务后缀，但仅适用于型号。**
7.  没有这个属性它也能工作。但是如果这个属性被设置为 true，这将生成一个 indexFile，它引用所有您生成的文件，并且可以添加到 tsconfig.json 中。

## 4.第四步

既然我们已经得到了我们需要的一切。

在您的终端中运行此命令。

```
ng-openapi-gen -c apps/savings-presentation/generate-typings/test/test-config.json
```

瞧啊。！现在您可以看到在 generate-typing/API 目录中生成的所有文件。

## 5.第五步

现在让我们将它连接到您的应用程序。

您将看到一个生成的模块“GeneratedApiModule”。现在将它导入 app.module.ts。

**注意:**生成的服务将拥有到每个控制器/资源的端点的路径，但是您仍然必须定义您的 API 的域。进行以下更改，您应该能够使用这些服务进行 http 呼叫。

```
GeneratedApiModule.forRoot({
  rootUrl: `${environment.apiServer.domain}/<path>`
})
```

现在，我们都准备好了。只需使用服务并调用必要的方法来无缝调用 API，而不必担心 UI 会与后端不同步。

**谢谢你，编码快乐！！**

**接下来:**

1.  如何将其扩展到多环境和 Monorepo 结构，并将其集成到 angular build 命令中。
2.  如何使用生成的服务进行 api 调用。
3.  将此整合到 datorama/akita，使其在国家管理方面更加强大。