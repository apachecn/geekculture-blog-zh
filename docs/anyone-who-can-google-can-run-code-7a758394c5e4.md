# 任何会谷歌的人都可以运行代码。

> 原文：<https://medium.com/geekculture/anyone-who-can-google-can-run-code-7a758394c5e4?source=collection_archive---------20----------------------->

![](img/2ad433a4403036c5595d6d2a98541f58.png)

代码现在无处不在。你可以通过搜索“code to <do-this>”找到大量的网络生活窍门。</do-this>

NodeJS 是在您的计算机上执行单文件程序的好方法。执行一个”。js”文件与单击软件中的按钮没有什么不同。你可以灵活地做一些小的改变，一切都在你眼前发生，而不是被用户界面所掩盖。

这是一组简单的说明，用于学习如何在计算机上安装 Node 和运行 NodeJS 程序。这是一个两分钟的任务，将为你打开一个充满可能性的世界。

# 苹果个人计算机

1.  前往 Mac 上的终端(Command +Shift，搜索终端)。
2.  运行以下命令。

```
> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

让它运行并结束。这将安装家酿。[家酿](https://brew.sh/)是一个 Mac 的软件包管理器。

3.现在安装节点。在命令下运行。

```
> brew install node
```

这将安装“节点”和“npm” [NPM](https://www.npmjs.com/) 是 NodeJs 的包经理。

确保通过在终端提示符下执行这些命令来安装它们。

```
node -v
npm -v
```

4.通过 finder 打开您的 **Documents** 目录中的一个文本文件。把下面的内容粘贴进去。另存为***icaruncode . js .***

```
console.log("Yay! I can run code!");
```

5.现在回到终端粘贴这个命令。

```
cd ~/Documents
```

这将把终端中的目录更改为您的文档目录。通过将 **/ <文件夹名>** 附加到上述命令，您可以更改在您的文档文件夹中添加任何文件夹。确保将其更改为保存 icanruncode.js 文件的文件夹名称。

6.假设终端提示符现在位于文件所在的目录中，执行以下命令。

```
node icanruncode.js
```

这应该打印出“耶！我会跑代码！”在控制台里。

恭喜你。您现在可以运行代码了。

7.要安装任何节点包，只需执行以下命令。

```
npm i -g <package-name>
```

例如，要安装[木偶师](https://pptr.dev/)，使用以下命令。

```
npm i -g puppeteer
```

等待安装完成。现在您可以执行任何需要运行木偶包的程序了。

**提示**:在 NodeJS 中，包的依赖关系通常是这样列在顶部的:

```
const puppeteer = require("puppeteer");
```

这意味着你需要安装“木偶师”包来运行程序。

# Windows 操作系统

1.从[https://nodejs.org/en/download](https://nodejs.org/en/download/)/下载 NodeJS 安装程序并安装。这将在您的计算机上安装节点和 npm。 [NPM](https://npmjs.com) 是 [NodeJS](https://nodejs.org) 的包经理。

2.打开 windows 终端或 powershell。

*搜索> cmd >右键>以管理员身份运行*

或者

*搜索> Powershell >右键>以管理员身份运行*

3.通过在终端提示符下执行这些命令，确保 Node 和 npm 都已安装。

```
node -v
npm -v
```

4.打开您选择的文本编辑器。将下面的代码粘贴到其中。并保存为**icaruncode . js**。

```
console.log("Yay! I can run code!");
```

5.在终端中，执行以下命令。确保将<path-to-the-file>部分更改为文件的实际路径。如果你找不到你的路径，请看[这篇文章](https://www.sony.com/electronics/support/articles/00015251)寻求帮助。</path-to-the-file>

```
node \<path-to-the-file>\icanruncode.js
```

这应该打印“耶！我会跑代码！”在你的控制台里。

恭喜你。您现在可以运行代码了。

7.要安装任何节点包，只需执行以下命令。

```
npm i -g <package-name>
```

例如，要安装[木偶师](https://pptr.dev/)，使用下面的命令。

```
npm i -g puppeteer
```

等待安装完成。现在您可以执行任何需要运行木偶包的程序了。

**提示**:在 NodeJS 中，包依赖关系通常是这样列在顶部的:

```
const puppeteer = require("puppeteer");
```

这意味着你需要安装“木偶师”包来运行程序。

仅此而已！