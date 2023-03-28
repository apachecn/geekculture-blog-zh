# 将 visual studio 代码配置为默认的 git 编辑器、比较工具或合并工具

> 原文：<https://medium.com/geekculture/configure-visual-studio-code-as-a-default-git-editor-diff-tool-or-merge-tool-291fd7088cc9?source=collection_archive---------1----------------------->

![](img/89c7629a844f865410677c37fbc16fb9.png)

Photo by [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我使用 visual studio 代码作为很多机会的固定工具，比如快速打开和编辑文件和源代码，因为它对我来说真的很轻很快。过了一段时间，我看着 VS 代码并思考，为什么我不用它作为一个 g it 工具来编写提交消息，获得文件之间的差异，或者解决合并冲突。

所以我发现 VS 代码可以被配置成 git 中一个非常方便的工具。

# 从命令行运行 VS 代码

在将 VS 代码用作 git 编辑器之前，必须确保可以从命令行访问和运行它。您可以通过运行下面的 CLI 命令来测试 access VS 代码。

```
$ code --version
1.66.0
e18005f0f1b33c29e81d732535d8c0e47cafb0b5
x64
```

如果找不到该命令，您必须将 VS 代码引入您的操作系统，以便可以从 CLI 运行它:

*   Windows:必须将 VS 代码安装的位置添加到环境变量设置中的 PATH 变量中。
*   Mac OS:按下`CMD+Shift+P`键，在命令面板中键入`Shell Command: Install 'Code' command in PATH`。
*   Linux:您必须从`.deb`或`.rpm`包文件中安装 VS 代码。

# 使用 VS 代码代替 nano 作为默认的 git 编辑器

默认的 git 编辑器是 Nano，我曾经使用 vim 作为 git 编辑器，直到我发现 VS 代码对此很方便。您可以使用下面的命令将 VS 代码配置为默认编辑器:

```
$ git config --global core.editor "code --wait --new-window"
```

这个命令在一个新窗口中打开 VS 代码，git 一直等到保存内容并关闭窗口。您可以通过以下命令删除 VS 代码:

```
$ git config --global --unset core.editor
```

# 使用 VS 代码作为 git 默认的比较工具和合并工具

默认的 git diff 工具是 vimdiff，您可以通过下面的命令设置它:

```
$ git config --global diff.tool vscode
$ git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'
```

您可以通过以下命令取消设置这些配置:

```
$ git config --global --unset diff.tool
$ git config --global --unset difftool.vscode.cmd
```

Git 没有默认的合并工具，您也可以使用 vimdiff 作为合并工具，或者通过下面的命令将 VS 代码设置为默认:

```
$ git config --global merge.tool vscode
$ git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

您可以像 diff 工具一样，通过在 config 命令后使用`--unset`标志来取消设置这些配置。

经过这些更改后，我的全局 git 配置文件如下所示:

```
$ cat ~/.gitconfig
...
[merge]
	tool = vscode
[core]
	editor = code --wait --new-window
[difftool "vscode"]
	cmd = code --wait --diff $LOCAL $REMOTE
[diff]
	tool = vscode
[mergetool "vscode"]
	cmd = code --wait $MERGED
...
```

您可以将所有这些行复制到主目录(`~/.gitconfig`)中的 git 配置文件中，以设置上面的所有命令。

# 结论

我总是更喜欢使用默认的 CLI 工具，如 vim，来使用 git 和其他命令行工具，因为有时您需要在只安装了默认工具的无头服务器中使用 git 和 bash，所以您首先需要知道如何使用这些工具。另一方面，GUI 工具帮助我们更快地做事情，不会在复杂的情况下感到困惑；VS 代码是最好的免费工具之一，你可以在你的开发环境中用作编辑器和比较工具。

# 资源

*   [git 官方文档](https://git-scm.com/doc)