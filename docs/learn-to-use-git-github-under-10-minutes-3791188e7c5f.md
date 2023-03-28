# 学习使用 Git & GitHub |不到 10 分钟

> 原文：<https://medium.com/geekculture/learn-to-use-git-github-under-10-minutes-3791188e7c5f?source=collection_archive---------32----------------------->

![](img/1e1e424288dd58d4d0a9a4fb6ed67c78.png)

Photo by [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

以下是您将在本教程中学习的所有内容:

*   通过 HTTPS 派生和克隆远程存储库。
*   将上游存储库添加到本地开发环境中。
*   创建和使用分支。
*   了解项目开发和投稿工作流程。
*   学习使用 commit、add、push & pull 等基本 git 命令。
*   将更改推送到您的 fork，并创建一个对上游存储库的 pull 请求。

但首先，Git 到底是什么？这是一个版本控制软件，用于在您修改文件时跟踪更改。是的，就这么简单。您可以追溯到您的第一个`commit`文件，并查看您对文件所做的更改。

> 这是你能得到的最接近时间机器的东西了——但只适用于你的文件和文件夹。

那是一口，按照下面的步骤来练习吧！

如果你对本教程有任何问题，请对本文发表评论或在[资源库](https://github.com/hemanth-kotagiri/practice-git)提出问题。我会尽快回复你的！

# 派生和克隆远程存储库

在开始使用 GitHub 之前，您需要有一个帐户。[在 GitHub 注册](https://github.com/join)然后回到这里。现在，转到这个链接，按照这些步骤在你的计算机上安装 Git:[点击这里](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。如果您使用的是 Linux，您可以使用您的包管理器直接安装 git。

要检查是否在本地计算机上安装了 Git，请在命令提示符/终端中键入以下内容:

```
git --version
```

如果你做的一切都是正确的，你将会得到你正在运行的 Git 版本的提示。如果没有，请单击上面提到的链接并正确安装。

安装完 Git 后，您需要克隆这个库。为此，请遵循以下步骤:

*   点击您在[右上角看到的`fork`按钮，这是](https://github.com/hemanth-kotagiri/practice-git)存储库。
*   这将在您的帐户中创建一个存储库副本。
*   点击你看到的绿色代码按钮，你会得到一个下拉菜单，并复制指定的链接。
*   现在，打开您的命令提示符/终端，通过粘贴您复制的链接来执行以下命令:

```
git clone [https://github.com/{YOUR_GITHUB_ID}/practice-git.git](https://github.com/hemanth-kotagiri/practice-git.git)
```

这样，您现在就有了存储库的本地副本。

# 将上游存储库添加到本地开发环境中

将上游(引用您已经克隆的原始存储库的一种奇特方式)添加到您的本地副本是非常必要的。为此，请按照下列步骤操作:

*   确保您位于克隆的存储库的目录中。
*   键入以下命令:

```
git remote add upstream [https://github.com/hemanth-kotagiri/practice-git.git](https://github.com/hemanth-kotagiri/practice-git.git)
```

*   现在，上游存储库将被添加到您的本地开发环境中。
*   要检查您拥有的可用遥控器，请键入以下内容:

```
git remove -v
```

您将得到以下输出:

```
origin	https://github.com/{YOUR-GITHUB-ID}/practice-git.git (fetch) origin	https://github.com/{YOUR-GITHUB-ID}/practice-git.git (push) upstream	https://github.com/hemanth-kotagiri/practice-git.git (fetch) 
upstream	https://github.com/hemanth-kotagiri/practice-git.git (push)
```

您现在已经准备好进入下一步，即创建一个新的分支。

# 创建和使用分支

分支实际上就是工作流的分支。您的存储库可以有多个分支，其中每个分支可以有自己的名称，并且可能有与主分支不同的代码。分支的想法是从主/主分支中脱离出来，修补和处理新的想法/实现，并使用该分支来引用提升 PRs 和`merging`主/主分支的新变化/实现。按照这些步骤创建一个新的分支并签出(一个有趣的术语来说激活)它。要了解更多关于分支机构的信息，请前往[官方文档](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches)。

假设您位于克隆存储库的同一个工作目录中，请在命令提示符下键入以下内容:

```
git branch feat-0
```

*   这里，`feat-0`是您刚刚创建的分支的名称。
*   要查看您拥有的所有分支:

```
git branch -a
```

*   现在，要激活该分支，请键入以下内容:

```
git checkout feat-0
```

现在会提示您已经成功地将您的分支机构更改为`feat-0`

# 了解项目开发和贡献工作流

问:为什么我们要创建一个新的分支机构，为什么不在主分支机构上工作呢？

将您正在处理的特性/实现模块化总是一个好的做法，这样它们就可以被在同一个项目中与您一起工作的同事单独处理。

作为任何开源项目的贡献者，你都需要遵循一套贡献指南。创建一个分支，并使用它提高 PRs 是绝对必要的，这是我们在这里讨论的。通常，对于任何项目，这些都是向存储库中添加新特性/代码所采取的步骤。

*   开发人员将在他们的帐户中维护一个上游存储库的分支。
*   然后他们用一个有意义的名字创建一个新的分支。
*   通过添加新代码/实现在本地进行更改。
*   然后，他们用一个有意义的消息提交他们的更改，并将其推送到他们的远程源。
*   之后，开发人员将着手创建一个新的对上游存储库的拉请求。
*   来自上游存储库的维护者将被通知，变更将被审查和接受/拒绝。

你不用担心最后一步！

# 学习使用基本的 git 命令，如:`commit`、`add`、`push`、&、`pull`

这是您需要熟悉的四个基本命令。让我们以一种有趣的方式来学习这四个命令。请遵循以下步骤:

还记得我们添加了一个上游存储库吗？现在，是时候使用`pull`命令将该存储库与您的本地副本同步了。

## 拉

在命令提示符/终端中键入以下内容，并确保您位于存储库目录中:

```
git pull upstream master
```

问:上面的命令有什么作用？

它将`pull`将`upstream`(您分叉的原始存储库)`master`的变更/代码/实现分支到您的本地副本中。

现在复制您的存储库 URL 并键入以下命令:

```
git remote add origin {YOUR-REPOSITORY-URL}
```

如果根本不存在的话，以上将创建原点。通过这种方式，您将从一个远程存储库中获取变更，无论是从上游还是从您的起点。在 origin 的情况下，您应该键入:`git pull origin master`

## 添加并提交

现在，上游存储库中的所有变更都与您的本地副本同步了。现在，您可以简单地在存储库中创建一个文件并跟踪它的变化。遵循这些:

*   在目录中用你的名字创建一个新文件夹。
*   将目录切换到您刚刚创建的文件夹中。
*   现在，轮到你跟踪任何你想跟踪的文件了。它可以是由一首诗、一个故事、一段引语或更专业的代码组成的文件。
*   创建一个新文件，并在其中写入任何内容，并确保它保存在您创建的文件夹中。
*   现在，您已经为第一次提交做好了准备。

## 了解临时区域

在命令提示符/终端中键入以下内容时:

```
git status
```

系统将提示您有一个未被跟踪的文件。这是您将它移动到准备区域的地方，也就是说，它们已经准备好提交了。

使用`add`命令通过以下命令添加要跟踪的特定文件:如果您已经将新文件命名为`poem.txt`:

```
git add poem.txt
```

现在，如果您键入这个:`git status`，您将看到文件已经准备好提交。键入以下内容:

```
git commit -m "{MESSAGE}"
```

使用上面的命令提交更改，并显示一条模糊描述您的更改的消息。

## 推

现在，键入以下内容，最终将您的更改推送到远程分支

```
git push -u origin feat-0
```

至此，您已经成功推送了新文件。要查看更改，请前往 GitHub 中的存储库，您会看到有一个包含新文件的新分支。

# 创建拉取请求

要创建一个 pull request，点击“Create Pull Request”按钮，当你访问 GitHub 中的库时，系统会提示你。您现在可以添加标题和描述，您可以在 Markdown 中编写，或者纯文本也可以。解释完更改后，单击 Create 按钮，就完成了！

现在，知识库的维护者将会收到关于你的 PR 的通知，他们将会检查它！

同样，如果您对本教程有任何疑问/问题，请在存储库中发表评论或提出问题。并且一如既往的记住“你可以学任何东西”！

> 对宝贝，用耐心