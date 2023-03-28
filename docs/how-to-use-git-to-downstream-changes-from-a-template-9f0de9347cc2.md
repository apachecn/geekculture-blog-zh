# 如何使用 Git 对模板进行下游更改

> 原文：<https://medium.com/geekculture/how-to-use-git-to-downstream-changes-from-a-template-9f0de9347cc2?source=collection_archive---------9----------------------->

![](img/f5869f729b60d6d3e7bf9c0ab39d7eb3.png)

[Jason Long](https://twitter.com/jasonlong) — [http://git-scm.com/downloads/logos](http://git-scm.com/downloads/logos)

上周，我不得不在一个模板中更新 Gradle Enterprise 的版本，并对其所有已实现的项目进行下游更改。我很好奇这有多有用，尤其是如果你有很多项目都是在同一个基础上构建的。我只需要更新模板和下游的变化，但在这篇文章中，我还将描述如何创建这些模板之一，以及如何在项目中使用它。

# 为什么要使用模板

模板的目的是在整个代码库中共享样板代码，这减少了启动新项目所需的时间，让您有更多的时间关注重要的东西。更多信息[这里](/programming-lite/repository-templates-in-github-why-how-75b1d1bd43f2)。

# 创建模板

首先，我们需要在我们最喜欢的平台上创建一个新的 git 库，比如 GitHub、Bitbucket 或 GitLab。现在，我们可以将空的存储库克隆到本地机器上，并在模板中添加我们想要的文件。

```
$ git clone <link to the repo>
```

然后，我们只需再次将更改推送到遥控器。

```
$ git add .
$ git commit -m "Initial Commit"
$ git push
```

祝贺您，您刚刚创建了您的第一个模板

# 设置项目

创建模板后，我们可以为我们的项目创建一个新的 repo。完成后，克隆模板回购，但使用不同于标准回购的名称。

```
$ git clone <link to the template> <name of the project>
```

Git 仍然会用指向模板的上行和下行 URL 创建远程源配置。

```
$ git remote -v
origin <link to the template> (fetch)
origin <link to the template> (push)
```

由于我们想推送到我们自己的项目，而不是模板本身，我们需要重命名原点。

```
$ git remote rename origin upstream
```

这样，推送和获取变更的默认位置就是项目存储库。目前，我们仍然可以通过以下方式对模板进行修改:

```
$ git push upstream
```

这应该是不可能的…要改变这一点，我们可以将 push upstream 的 URL 改为`no_push`。

```
git remote set-url --push upstream no_push
```

如果我们现在使用`git remote -v`命令，我们将看到类似这样的内容:

```
$ git remote -v
origin <link to the template> (fetch)
origin <link to the template> (push)
upstream <link to the template> (fetch)
upstream no_push (push)
```

如果我们现在试图将一些东西推到上游，我们会得到一个异常，告诉我们这个存储库不存在。

```
$ git push upstream
fatal: 'no_push' does not appear to be a git repository
fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.
```

# 模板的下游

首先，我们必须获取遥控器的最新变化:

```
$ git fetch 
```

接下来，我们可以合并变更，解决冲突(如果有的话)并推进到项目中。

```
$ git merge upstream/master
$ git add .
$ git commit -m "Downstreamed changes from template"
$ git push 
```

现在，您已经成功地将模板的更改下游化到您自己的项目中。恭喜👍 😸

# 反射

## 什么进展顺利

由于这是一个非常简单且众所周知的任务，我毫不费力地找到了关于这个主题的[好资源](https://www.mslinn.com/blog/2020/11/30/propagating-git-template-changes.html)。

## 什么需要改进

这与下游本身没有直接关系，但是将来我会对其他项目版本的依赖性做更多的研究。我正在升级的项目与另一个项目的特定版本有一些依赖关系。我不知道那件事，并且一直对错误感到疑惑，直到我向我团队中的某个人寻求帮助。