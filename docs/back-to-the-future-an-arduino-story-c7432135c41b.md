# 回到未来:一个 Arduino 的故事

> 原文：<https://medium.com/geekculture/back-to-the-future-an-arduino-story-c7432135c41b?source=collection_archive---------6----------------------->

## 一位“电子工程师出身的 UX 设计师”在离开电子世界 12 年后，如何与他的孩子一起构建了一个微控制器电路！

![](img/3d3cfba214091c2c78740de99b5a4c2f.png)

让我们回到 30 年前开始这个故事…

# 一个 12 岁的电子爱好者

90 年代初。我在我们印度小镇的一所学校上七年级。一天，住在附近的朋友谢哈布来到我家，手里拿着一本名为《人人用电子产品》的杂志。它的特色是业余爱好电路，关于电子学基本原理的文章，业余无线电，电子零件供应商的联系方式…

Shihab 带给我的杂志有一个非常简单的音乐门铃项目，使用的是 [UM66 集成芯片](https://pdf1.alldatasheet.com/datasheet-pdf/view/113485/UMC/UM66T.html) (IC)。从我们镇上的一家电子商店买了集成电路、几个电阻、一个晶体管和一个两英寸的扬声器后，我们只用连接线在一个旧的盒式录音带塑料外壳上组装电路。我们没有印刷电路板。我们不知道如何焊接这些部件。我们甚至没有必要的设备/工具，比如万用表、脱焊泵、镊子…

电路在第一次尝试中就自己工作了！路德维希·范·贝多芬的[《献给伊利斯》](https://upload.wikimedia.org/wikipedia/commons/7/7b/FurElise.ogg)通过微型扬声器传遍了整个房间。那是我第一次听到这首无处不在的名曲！这次经历让我变成了一个电子爱好者，我开始构建更多的电路——放大器、收音机、低频调幅发射机……

后来，我于 2000 年毕业，获得了电子工程学位，在电子、R&D 和汽车电子行业工作了近八年。

[然后，设计缺陷在 2008 年咬了我](https://designpuli.com/2008/05/21/everything-that-has-a-beginning-needs-a-start/)。在获得产品设计硕士学位后，我改变了我的职业，现在我是一名成功的 UX 设计师，已经工作了 12 年。

从 2010 年开始，我再也没有回头看电子。我完全退出了微芯片、Atmel、ON Semi、恩智浦、摩托罗拉、飞兆、德州仪器、R&S 的世界，没有关注那个领域发生的事情。

直到今天！

# 绝地归来

今年 1 月(2022 年)，我意识到我的孩子已经到了可以涉足基于微控制器的电子电路编程的年龄。我曾经见过很多基于 Raspberry Pi 的项目——但是经过一点点的研究，我放弃了它，因为它对我来说似乎很难掌握。我想要一个比基于微芯片 PIC 系统更实用的平台。最终，所有的路都将我引向 [Arduino](https://en.wikipedia.org/wiki/Arduino) 。

![](img/97a40dfbd8810457a3814dbd24483afb.png)

[Arduino Starter Kit K000007](https://store-usa.arduino.cc/collections/kits/products/arduino-starter-kit-multi-language)

我从 Smapptech Systems Pvt Ltd 购买了 [Arduino Starter Kit K000007](https://store-usa.arduino.cc/collections/kits/products/arduino-starter-kit-multi-language) ，根据 Arduino 网站显示，smapp tech Systems Pvt Ltd 是印度的[授权经销商。该工具包有一个](https://store-usa.arduino.cc/pages/distributors) [Arduino UNO 板](https://docs.arduino.cc/hardware/uno-rev3)和一个 [Arduino 项目手册(英文)](https://designpuli.com/wp-content/uploads/2022/05/00-Arduino_Projects_Book.pdf)一步一步地解释了大约 14 个项目。Arduino 的联合创始人之一 Massimo Banzi 在 YouTube 上提供了一些 Arduino 视频教程。

Unboxing our Arduino Starter Kit

构建这些项目所需的所有硬件组件(电阻、电容、16×2 LCD、压电蜂鸣器、伺服电机、DC 电机……)都已经包含在初学者工具包中。 [Arduino IDE(集成开发环境)](https://www.arduino.cc/en/software)是开源的，可以安装在计算机中，用于编写我们的 Arduino 电路的代码。在 Arduino IDE 中保存代码后，它会编译可执行代码，并通过 USB 电缆将其下载到我们的 Arduino UNO 板上。

带着新的希望和孩子们的鼓励，我开始重游我的旧“电子帝国”

# 一个“不可阻挡”的秒表！

在尝试了项目手册中的一些电路并取得一定成功后，我们想做一些新的、简单的事情；而是完全属于我们自己的东西。最后，我们决定用液晶显示器制作一个秒表。电路原理图是在参考了 Arduino UNO 电路板的引脚和 Arduino 项目手册中提到的其他组件后手绘的。我们的电路原理图有一个“暂停/恢复”按钮开关和一个压电蜂鸣器，每分钟发出一次蜂鸣声。

![](img/5127ba954d7c9458d2e9497e43889039.png)

Schematics Diagram of the Stopwatch Circuit that we are trying to build

在试验板上给电路布线并不需要太多时间。另一方面，想出一个可行的代码需要一些时间。经过多次迭代后，基本功能已经就绪，秒表以 MM:SS 格式正常工作，时间长达 59 分 59 秒。

最终，上面原理图中的蓝色部分没有建成。由于没有暂停/恢复按钮开关，我们的秒表在跑步时根本无法停止！

## 警告:前方有令人讨厌的东西…你可以跳过这一部分，进入下一部分！

由于 Arduino UNO 的外部中断引脚 D2 和 D3 被 LCD 数据线用完，我们无法将这些引脚用于暂停/恢复按钮开关。对按钮开关的按键状态进行轮询的另一种尝试完全破坏了秒表的准确性。因此，我们被迫删除暂停/恢复功能，导致无法停止秒表！现在秒表电路变成了 59 分 59 秒的递增计数器。然后，它将重置为 00:00，并再次开始向上计数。

来自压电蜂鸣器 Arduino 的脉宽调制(PWM)信号开始干扰 LCD 数据线，导致屏幕上出现乱码。通过将压电蜂鸣器接线从 LCD 线路移开以减少干扰，解决了这一问题。我们还将压电蜂鸣器发出“嘟嘟”声的持续时间从 1 秒减少到了 500 毫秒。

# 花在“小时”上的时间

当我们试图在 LCD 上以 HH:MM:SS 格式显示时间时，事情变糟了。秒表显示的最高时间是 23 小时 59 分 59 秒。然后我们遇到了一个障碍，一个难以捉摸的 bug 开始在跑步时重置秒表。

在努力了几个小时找出这个错误后，我完全失去了耐心。然后，我开始寻找一个板载调试器或 Arduino 模拟器，它将接近实时地打开运行代码的变量的状态和值。然后我有了惊人的发现！

# 救援用 TinkerCAD 电路模拟器

Autodesk 基于浏览器的 TinkerCAD 有一个优秀的电路模拟器，以及更受欢迎的 3D 建模模块。电路模拟器有一个装满电子元件和微控制器系统的库，包括商用 Arduino 板。这使得在您的计算机上虚拟构建和测试电子系统成为可能，而无需购买或组装相应的物理硬件。

而且，小叮当是免费的。你可以边做边学电子，同时还能省钱！

![](img/325b02f564890418dec35ac6d72a2468.png)

Our Stopwatch circuit reproduced in TinkerCAD Simulator

![](img/90222078c8e9baa2e14e48e78539eaa8.png)

Schematics Diagram automatically drawn by TinkerCAD based on the circuit created in the Simulator

在 TinkerCAD 模拟器中复制我们的秒表电路后，我们的电路软件代码也从 Arduino IDE 复制到其中。通过添加断点，单步执行每一行代码，并观察存储在不同变量中的值，我们能够重现错误。一旦问题被发现(重置某些变量时的愚蠢错误！)，找到解决方案，测试代码真的很快。TinkerCAD 模拟器也节省时间！

# 原力觉醒了

![](img/5e01feacf94779a2b457876d1d20c6b5.png)

Our Stopwatch with HH:MM:SS implemented

点击这里，你可以在 [TinkerCAD 模拟器中查看秒表电路的最终版本、代码执行和原理图。](https://www.tinkercad.com/things/gYuw0xS6kED?sharecode=FFnFTJ767dW9jGK4UW7UwxFOF8I6BKcim4qGLQAjRC4)

Play this video with unmuted sound to hear the Piezo Buzzer ‘beep’

我的孩子们对这个“新玩具”非常兴奋我也发现设计和建造一些有形的东西很有趣。在我孩子的学校六月开学之前，我们计划做更多有趣的 Arduino 项目。

我们现在要做的是制造一个智能茶杯垫。当热茶/咖啡冷却到你舒适、可饮用的首选温度时，它会提醒你。现在你不必因为喝太热的茶/咖啡而烫伤你的嘴和嘴唇。你也不必喝冰茶/咖啡，因为你等它凉下来的时间太长了。我的孩子每天都面临这个问题，因此有了这个产品创意！

![](img/2d9811305cb536caa88819b9a2baa4d1.png)

Coming soon! Smart Coaster Arduino Project

祝我们好运！

*我们知道其他人已经制造了完全相同的产品，有些人甚至使用 Arduino 电路。但我们不会偷看，也不会偷他们的东西。我们保证！*😊