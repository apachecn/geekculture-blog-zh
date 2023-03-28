# 了解 NodeJS 中的 package.json 和 package-lock.json

> 原文：<https://medium.com/geekculture/understanding-package-json-and-package-lock-json-in-nodejs-eb04cd43d379?source=collection_archive---------10----------------------->

# 目标

了解 **package.json** 和 **package-lock.json** 在 NodeJS 中的意义，并对其做一些实验。

# 先决条件

*   系统中安装了最新版本的 NodeJS

# 我们开始吧..

# 第一部分

## # 1.1)package . JSON 文件的需求是什么？

**package.json** 是一个清单文件，描述了与项目相关的元数据。

例如，项目名称、项目版本、作者、脚本、git 存储库链接等。

现在，您的项目可能依赖于其他一些项目。我们一般称这些“其他项目”为当前项目的**依赖关系**。这些依赖关系以及它们的版本也需要在 **package.json** 中提及。

## # 1.2)NodeJS 中的语义版本化(semver)是什么？

`npm`中的所有项目都遵循一个版本化系统，称为语义版本化，它规定一个项目应该以`x.y.z`格式维护它们的版本，其中—

*   x →主要版本
*   y →次要版本
*   z →补丁版本

当项目的一个依赖项被更新时，这些版本中的一个会增加到某个值，这取决于该依赖项的变化程度

*   当进行不兼容的 API 更改时，主版本会增加。
*   以向后兼容的方式添加功能时，次要版本会增加。
*   当有向后兼容的错误修复时，补丁版本会增加。

## #1.3)包版本的 semver 规则是什么？

假设，您选择了一个依赖项的最新版本，并开始在项目中使用它。让我们假设最新的版本是`3.1.0`。

你开始使用这种依赖，消费它的 API，让你的项目活起来。6 个月后，你会意识到这种依赖有很多`**patch version updation**`和`**minor version updation**`。

假设有`3.1.1`、`3.1.2`、`3.1.3`到`3.1.10`补丁版本更新，之后有`3.2.1`、`3.2.2`到`3.2.5`小版本和补丁版本更新。

您意识到，对于您的项目，只有当您将该依赖项更新到版本`3.1.0`即`3.1.10`的最新补丁时，只有该依赖项所使用的 API 才能正常工作。如果你一直更新依赖关系到`3.2.5`，那么一些错误就会出现。

> 那么，如何告诉`npm`将依赖项更新到它的最新补丁版本，而不是它的最终最新版本呢？
> 
> 答案是`semver`规则。

其中一些规则是—

*   `~` →如果在`package.json`中以这种方式指定了我们上面例子的依赖项的当前版本——`~3.1.0`，那么在运行`npm update`时，该依赖项的版本将被更新为`3.1.10`，即它将只更新其补丁版本。对于上面的查询，这是推荐的方法。
*   `^` →如果在`package.json`中以这种方式指定了我们上面示例的依赖项的当前版本— `^3.1.0`，那么在运行`npm update`时，该依赖项的版本将被更新为`3.2.5`，即它将更新其次要版本以及补丁版本。

还有其他规则。你可以参考[这个链接](https://nodejs.dev/learn/semantic-versioning-using-npm)了解那些规则。

## #1.4)只有 package.json 有什么问题？

*(以下示例参考了#1.3 中提到的示例)*

假设在 2021 年 1 月 1 日，您在您的 git 存储库中推送一个工作项目，并且该项目利用了一个依赖项，该依赖项的版本在您的`package.json`中被指定为`^3.1.0`。假设还没有`package-lock.json`。

6 个月后，其他人克隆了这个库，并做了`npm install`来下载依赖项。那个人会注意到，由于 NodeJS 中的`semver`规则，实际安装的依赖项的版本是`3.2.5`。

> 那不对，对！！？

因为我们希望无论是谁克隆存储库并执行`npm install`都应该安装与项目实际创建时相同的依赖版本，这样项目才能正常工作。

这就是`package-lock.json`出现的原因。

> 如 [npm 文档](https://nodejs.dev/learn/the-package-lock-json-file)中所述——
> 
> `package-lock.json`文件的目标是跟踪每个被安装的包的确切版本，这样即使包被他们的维护者更新，一个产品也能以同样的方式 100%可复制。
> 
> `package-lock.json`将你当前安装的每个包**的版本固定在**中，`npm`将在运行`npm install`时使用那些确切的版本。

```
Note : Find out the difference between "npm install" and "npm ci" . 
```

# 第二部分

在本节中，我们将通过实验来了解更多关于`package.json`和`package-lock.json`文件的行为。

让我们先设定实验的基础。

执行以下命令:

Gist #2.1 — Command Set 1

在最后一个命令的末尾，您会看到版本为 9.0.1 的`@angular/cli`包已经安装在项目中。

检查一下`package.json`文件。你会看到这个版本被称为—

```
"dependencies": {
    "[@angular/cli](http://twitter.com/angular/cli)": "^9.0.1"
 }
```

结账`package-lock.json`也一样。

你一定也看到了 node_modules 文件夹。在将第一次提交推送到远程存储库之前，您应该排除这种情况。

补充。`gitginore`文件，键入`node_modules`并保存。

现在执行以下命令—

Gist #2.2 — Command Set 2

我们的基地已经准备好了！

> **您也可以通过此链接克隆基本存储库**
> 
> 【https://github.com/ks-mani/package-json-exp】
> 
> **在下面的实验中，为了方便起见，我将只克隆这个回购。**

让我们做一些实验。

## #2.1)实验 1

在这种情况下，我们将把 repo 克隆到一个单独的文件夹中，执行`npm install`并检查已安装的依赖项的版本。

Gist #2.3 — Command Set 3

执行这些命令后，您将看到安装的@angular/cli 的实际版本是 9.0.1。

这意味着该项目是 100%可复制的原始项目。

## #2.2)实验 2

在这种情况下，我们将把 repo 克隆到一个单独的文件夹中，执行`npm install`并检查已安装的依赖项的版本。

之后我们再做`npm update`，看更新版。

Gist #2.4 — Command Set 4

执行完命令后，您会发现依赖关系的 semver 符号保持不变，即`^9.0.1`，但是`package-lock.json`中提到的实际版本变成了`9.1.13`。

因此，它显示了预期的行为。

## #2.3)实验 3

在这里，我们将把回购克隆到一个单独的文件夹中，删除`package-lock.json`文件，然后执行`npm install`。

Gist #2.5 — Command Set 5

执行完命令后，我们观察到`package-lock.json`文件(在`npm install`之后重新生成)中安装的依赖版本是`9.1.13`，而不是`9.0.1`。

这意味着如果没有`package-lock.json`文件，该项目就不能像原始项目一样 100%可复制。

# 结论

在本文中，我们看到了节点项目的`package.json`和`package-lock.json`文件的重要性。

更多信息可以参考 [NodeJS 学习章节](https://nodejs.dev/learn/an-introduction-to-the-npm-package-manager)。

我恳请您分享您对本文的反馈和建议。