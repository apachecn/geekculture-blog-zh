# Git 及其基本命令

> 原文：<https://medium.com/geekculture/git-you-shouldve-learned-it-yesterday-1f01df0badbd?source=collection_archive---------23----------------------->

![](img/c772faeb4b68eeb4ed379e0e17998f2c.png)

Photo by [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是一个有抱负的软件开发人员，你一定遇到过 Git(或任何其他版本控制系统，如 Mercurial、Subversion 等。).

当在一个单独的项目中工作时，你可能觉得没有必要使用任何版本控制系统，但是当你在一个团队中工作时，或者与不同的合作者一起工作时，事情就完全变了。

在这篇文章中，我将介绍一些基本的 git 命令。

我们开始吧！

# Git 是什么？

Git 是一个版本控制系统，这意味着它跟踪你和其他人在项目中所做的所有变更。它帮助您处理单个分支，将代码恢复到以前的状态，比较特定时间段内的代码，跟踪每个人所做的更改，等等。

# 入门指南

```
git init
```

该命令创建一个空的 git 存储库或重新初始化一个现有的存储库。使用终端导航到项目目录并运行该命令，现在您将能够使用该目录作为 git 存储库。

> git 初始化

```
git clone <repo url>
```

此命令用于创建副本或将远程存储库克隆到本地计算机上的新目录中。`<repo url>`是远程托管项目的远程位置的 URL(比如在 github.com 上)。

`remote`是远程托管项目的位置。

> git 克隆[https://github.com/pundirAbhishek/Git-Tutorial.git](https://github.com/pundirAbhishek/Git-Tutorial.git)

# 存储您的更改

完成必要的更改后，您现在可以将这些更改添加到远程分支。

```
git status
```

该命令显示工作目录和登台区的状态。它可以让您看到本地存储库中哪些文件已经被修改、添加或删除。

`git status`不显示任何关于已提交项目历史的信息。

> git 状态

```
git add *
```

该命令将工作目录中的更改添加到临时区域。临时区域是工作目录和远程目录之间的缓冲区。git 不是直接将更改推送到远程，而是首先将更改保存在临时区域中，如果我们想在将这些更改推送到远程之前进行任何更改的话。如果我们想对文件进行任何更改，我们可以轻松地将文件卸载。

如果我们只想添加一个文件，可以将`*`更改为一个特定的文件名；如果我们想一次添加所有文件，可以使用`.`。

> git 添加文件名
> 
> git 添加。

```
git commit
```

该命令获取临时区域中的所有文件，并保存要推送到远程设备的更改。我们还可以在提交的同时给出一条消息，这有助于确定我们正在推动的变更，并为我们以及当前从事该项目的其他人或将来从事该项目的其他人提供未来参考。

> git commit -m“此处显示提交消息”

```
git push <remote> <branch-name>
```

此命令用于将本地存储库的更改更新到远程存储库。推送是我们将提交从本地存储库转移到远程存储库的方式。`<remote>`是项目所在地，一般为 origin。`<branch-name>`我们要推进变更的分支的名称。

> git 推送原始主机

```
git stash
```

这个命令会暂时搁置我们工作库中的更改，以便我们可以处理其他内容，然后从我们停止的位置继续。`git stash`获取我们未提交的变更，保存它们以备后用，然后将它们恢复到我们的工作目录中。

> git 贮藏

# 管理分行

当多个合作者参与一个项目时，分支是有用的。每个人都可以在他们的分支上工作，这取决于他们正在工作的特性等。然后，再将每个分支合并成一个主分支。

```
git branch
```

该命令列出工作目录中所有可用的分支，并突出显示当前工作分支。当我们需要查看我们当前工作的分支或者我们想要切换到的目标分支的名称时，这是非常有用的。

> git 分支

```
git branch <branch-name>
```

这个命令在您的工作目录中创建一个名为`<branch-name>`的新分支。但是你现在的工作分支不变。

> git 分支 branchName

```
git checkout <branch-name>
```

这个命令将您当前的工作分支更改为您机器上已经存在的名为`<branch-name>`的分支。

> git 签出分支名称

```
git checkout -b <branch-name>
```

上面的命令帮助用户从当前分支创建一个名为`<branch-name>`的新分支，并使其成为当前工作分支。该命令将`git branch`和`git checkout`的功能合并成一个命令。

> git checkout -b branchName

```
git pull <remote> <branch-name>
```

上面的命令让用户从远程获取分支`<branch-name>`上的所有变更，并将它们合并到本地存储库中，并将变更合并到当前分支中。这有助于我们用其他人可能已经做的更改来更新我们的本地存储库。这个操作有时可能会导致`merge conflicts`，这意味着同一个文件或代码片段在另一个分支中也被修改了，用户必须检查哪些更改将被保留。

> git 拉原点分支名称

```
git merge <branch-name>
```

上面的命令有助于将来自另一个分支的更改集成到您当前的分支中。它将结合从`<branch-name>`分支到当前工作分支的所有变更。与`git pull`命令相同，该命令也可能导致`merge conflicts`，我们必须在合并成功之前解决这些冲突。

> git 合并分支名称

# 结论

这不是一个详尽的命令列表，而是我在工作中每天使用的一些命令。

请分享你的想法和建议。