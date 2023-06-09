# 玩 6502

> 原文：<https://medium.com/geekculture/playing-with-the-6502-2d9708ccc636?source=collection_archive---------16----------------------->

许多年前，我开始为一家名为“人力”的临时公司工作。他们问我是否能替换电路板上有标记的芯片，我在邮局当过电信学徒，可以做到。

我最终在里克曼斯沃斯的 Burroughs 气体等离子显示器部门工作。

我的第一天相当冒险，敲了敲办公室的门，一个人应了一声"是"，我伸出手和他握手，说"我是你的新临时工"。他基本上站到一边，一个完成人员走了进来，护送我到“实验室”，递给我一张气体等离子显示器上的规格表，上面写着“请阅读，我很快回来，并向您询问有关情况”，然后离开。

大约 10 分钟后，他回来了，兴高采烈地问我“它是如何工作的”，所以我告诉他，“你显然比我们预期的要熟练一点，你会修理 PCB 吗？”“如果你有示波器和图表，我可以试一试。”

六个星期后，他们决定不再为临时工付钱，他们给了我一个永久的职位，我欣然接受了。那个终点的家伙(我怀疑拼写正确)朱卡·哈卡拉只在英国工作，所以他的简历上有一份英文工作。他毕业于诺基亚大学，拿的是全额奖学金，大材小用了。

几个月后，我们收到了美国工厂的间接投诉，显然，我们用三分之一(2)的员工维修了三倍数量的显示器！有人建议我们放慢速度，这样下午我就不用修显示器了，而是亲自接受微处理器电路设计的辅导，比大学还好！

在 Burroughs 关闭显示器部门后，我成了一名合同设计工程师。那是 80 年代初，没有现成的操作系统，必须直接在“裸机”上写代码

很多有趣的故事，比如我用辛克莱光谱和一大块 sov Vero 板做了一个冰！如果有足够的兴趣，也许我会写一些。

现在已经到了退休年龄，在 Java 方面已经有 20 多年的经验，包括 C、C++、Visual Basic、Forth 和 Delphi 等等，不包括 TMS 9900 等处理器。我渴望做一些焊接工作😊

为了提高我的 Scala 知识，我写了一个应用程序，打算用来制作 6502 机器码编程的培训材料。该应用程序现在在 https://bit.ly/3PkAQZB 的 Git Hub 上运行发行版和源代码

我在网上找到了一个很好的静态版本的 6502，时钟一直到 50 兆赫，Commodore 64 只有 1 兆赫，为此写了几个游戏。

所以我想用它给自己建一个漂亮的小板子，并把它连接到当前的应用程序上。

当我写这些 Commodore 64 游戏时，我使用了一个 Atari ST，它带有一个交叉汇编器和一个通过串行接口连接的 64 上的小监视器应用程序——当然，我可以用 USB 芯片和一点逻辑做类似的事情，无论如何，找到它会很有趣。

app 代码是 Scala 3 使用 ScalaFX (JavaFX wrapper)进行 UI。

这是一个从一开始就学习的项目，如果你看到它，我希望你能学到，如果你有任何意见，我会学习，这将是我们两个的好日子。

不要工作——享受乐趣并获得报酬——成为一名开发人员！