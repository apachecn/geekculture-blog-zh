# 让水像瀑布一样流淌。

> 原文：<https://medium.com/geekculture/let-the-water-flow-like-a-waterfall-e8f8a10c7e7?source=collection_archive---------23----------------------->

## 目的:介绍 unity 中的 ***动画磁贴。***

拥有一个移动的世界可以让一个世界比到处都是静止的图像更有活力，它可能很微妙，但通过一些恰当的动画，它可以让一个场景变得生动。微弱的闪光或火焰，洞穴中的瀑布，甚至是随风摇曳的草。

![](img/9902b919829a96708055d1c631ccf4f5.png)

有了这个，我们将需要一些来自 unity 外包下载的帮助，谷歌“unity 2d tilemap extras”并打开 Github 链接下载代码，或者只需点击[这里](https://github.com/Unity-Technologies/2d-extras)。这是一个 unity 技术 Github，所以它应该可以安全下载。

![](img/2d528a5c70d6f6319a5abb869a28b5a9.png)

The web page should look something like this.

你可能需要查看分支才能获得在你的 unity 版本上工作的正确版本，我使用的是 2019.2.12f1，不得不下载 2019.4 版本，因为主版本在我的版本上不工作。

![](img/ffacbdbfc1ff4895fa52f08597a10418.png)

To get to this page, click the branches which is between “Master” and “Tags”

![](img/03543dd4cb61afdeb8eacddde02dfe98.png)

下载后，将文件夹移动到资产文件夹中。

![](img/f9de1ede3b11d5da607fca88f92e3a0d.png)

现在，当我们右键单击并选择创建，我们应该得到选项添加 2D >瓷砖>动画瓷砖。根据您下载的版本，它可能位于创建>平铺>动画平铺下

![](img/b229f4b34ac11e5496e869a9860bfd77.png)

完成所有这些工作后，我们就可以开始制作将要放入 tilemap 的 tile 了。首先，创建一个动画单幅图块，并将其与其他单幅图块一起保存在名为瀑布的新文件夹中。

![](img/f4696ce79672d8d17af807c6142d7eb4.png)

This animation is made up of 3 parts.

现在我们可以添加精灵。点击我们刚刚制作的动画精灵，你应该在检查器中看到这个。

![](img/ebdb85e494d6c528723b0d2f874823d6.png)

如果你有一个旧版本，你可能看不到拖动精灵框，这只是意味着它将需要一点时间。

![](img/0d4458989fde7ac4956f36c039ada2a8.png)

## 旧版本:

这里你需要为动画设置精灵的数量，并一个接一个地把它们拖到打开的地方。

![](img/e73a21396b104fe0163c76884f7db8a9.png)

## 较新版本:

这个简单多了，你可以选择所有的精灵并把它们都拖进来。只要记住锁定检查器，这样当你选择要添加的精灵时，它就不会关闭动画精灵。

瓷砖做好之后，我们现在可以开始制作调色板来存放瓷砖了。正如我们在上一篇文章中所做的，单击 create New Palette，并将其命名为瀑布。

![](img/fa51cb9fb9e90bada8241657993ecf99.png)

接下来拖动我们刚刚制作的动画瓷砖。

![](img/6051f5a280ea74ebc9d5c3cccb6d288f.png)

现在我们可以像绘制其他瓷砖一样将它们绘制到场景中。

![](img/3e9a820a84d475bac6363b335c5cd3d2.png)

要让它们有动画效果，你必须进入播放模式。

![](img/34c0eab6092e0e67023147b2b23172ab.png)

但是正如你所看到的，它移动得非常慢…我们可以通过返回到我们制作的动画瓷砖并更改它们的选项来解决这个问题。向下滚动到底部，直到你看到一个“最小速度”的选项，并将其设置为 30。完成后，带它去试运行。

![](img/df08439761b7bd9613ac8a4e3d829474.png)![](img/cb039ffc7c1581071a720ddc1130cd6d.png)

Speed-wise it’s better :)

制作瀑布的精灵中间有一部分看起来像一滴眼泪。我将不得不进入精灵，并做一些改变来修复它，但除此之外，它看起来不错。

![](img/18f00366615d4bf80688c88f02b56dae.png)

Right in the middle, top and bottom join perfectly though!

虽然精灵确实需要一些调整，但这是向 tilemap 添加动画精灵的基础。