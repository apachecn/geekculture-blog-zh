# NVM 如何安装 Node.js？

> 原文：<https://medium.com/geekculture/how-to-install-node-js-by-nvm-61addf4ab1ba?source=collection_archive---------1----------------------->

## 使用 nvm 安装和管理 Node.js 的多个版本。

有时，当我探索 GitHub 并克隆一些 Node.js 项目时，它与我当前安装的 Node.js 不兼容，我需要安装另一个版本。NVM 使它变得更容易，并允许我们在您本地机器上安装和管理 Node.js 的多个版本。

![](img/db1e6ee19b88717cee788871bb47cb5d.png)

Photo by [Ian Dooley](https://unsplash.com/@sadswim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 在 Ubuntu 和 Mac OS 上安装 nvm

有一些方法可以在你的机器上安装或更新 nvm，但是我更喜欢用[安装脚本](https://github.com/nvm-sh/nvm)来安装 nvm。我写这篇文章的时候，nvm 安装程序的最新版本是`0.38.0`。因此，您可以使用`curl`下载它，然后在终端中使用下面的命令用 bash 运行它。

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

或者用`wget`:

```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

之后，安装程序会将 nvm 库克隆到`~/.nvm/`目录，你应该在`~/.bashrc`文件的末尾添加几行来加载 nvm。

```
export NVM_DIR="$HOME/.nvm"
# This loads nvm
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
# This loads nvm bash_completion
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

现在您需要关闭并重新打开终端来加载 nvm，然后检查并验证您的安装

```
nvm --version
# 0.38.0
```

现在 nvm 已经可以使用了。

# 使用 nvm 管理 Node.js 版本

Nvm 有很多子命令，比如安装、使用、卸载等等。我们想选择一些必要的子命令并解释它们。

## 安装 Node.js 的版本

首先，您可以通过`list-remote`或`ls-remote`子命令获得可用版本的列表。

```
nvm list-remote # or nvm ls-remote
```

你可以用 nvm 的`install`子命令安装一个特定的版本。

```
nvm install <version> # like: 14.17.6
```

如果你想安装 LTS 版本，你可以用`--lts`代替版本号。

```
nvm install --lts
```

或者可以用`node`代替版本号安装最新版本。

```
nvm install node
```

## 加载 Node.js 的特定版本

现在，您在机器上安装了 Node.js 的一些版本；因此您可以看到带有`list`或`ls`子命令的已安装版本列表。

```
nvm list # or nvm ls
```

如果要加载特定版本，使用`use`子命令。使用这个子命令，可以通过版本号或`--lts`标志加载 Node.js。

```
nvm use <version>
```

或者使用 LTS 版本

```
nvm use --lts
```

或者使用最新版本

```
nvm use node
```

## 卸载 Node.js 版本

最后，如果您想卸载 Node.js 的一个版本，可以使用`uninstall`子命令。

首先，你需要用`use`切换到另一个版本，然后你可以卸载它。

```
nvm uninstall <version>
```

# 有什么？nvmrc 文件？

在 Node.js 项目中，你可以将 Node.js 的兼容版本存储在一个文件中，他们称之为`.nvmrc`，你应该在项目的根目录中创建它。

```
node --version > /path/to/project/.nvmrc
```

然后你就可以在队友的其他机器上使用兼容版本了。

```
nvm use
# Found '/path/to/project/.nvmrc' with version <v14.17.6>
# Now using node v14.17.6 (npm v6.14.15)
```

# 在 Node.js 上安装一个包

在 NVM 安装的 Node.js 上安装软件包与 node.js 的常规安装是一样的，但是软件包的安装是基于版本号的。

```
nvm use 14.17.5
npm install --global yarn
which yarn
# ~/.nvm/versions/node/v14.17.5/bin/yarn
```

# 结论

NVM 帮助我们将一些 node.js 版本放在一台机器上，并在我们的项目或测试开发版本的功能时使用它们，而不会对我们的系统或项目产生副作用。

因此，我推荐使用它来安装 Node.js 并轻松管理它。

# 资源:

[](https://github.com/nvm-sh/nvm) [## GitHub - nvm-sh/nvm:节点版本管理器-符合 POSIX 的 bash 脚本，用于管理多个活动的…

### nvm 是 node.js 的版本管理器，设计为按用户安装，按 shell 调用。nvm 可以在任何…

github.com](https://github.com/nvm-sh/nvm)