# 平滑移动——摆动使时间序列模型更有规律吗？

> 原文：<https://medium.com/geekculture/smooth-move-does-wiggling-make-time-series-models-less-accurate-8544e675873?source=collection_archive---------5----------------------->

这篇文章强调了流行的自主预测方法 Auto-ARIMA，Prophet 和 TBATS 的不连续性。我为 Python 用户提供了一个简单的单行“修复”(参见[笔记本](https://colab.research.google.com/github/microprediction/timemachines/blob/main/examples/notebooks/wiggling.ipynb?authuser=1#scrollTo=ktg4xxH57rNF)中的例子)。额外的规则性将花费计算，但不一定精确。

![](img/6ad5bdefda9d7fcef23c67bf52b5355e.png)

Because

# 一种自动 ARIMA 病理学

假设我给你一个 100 个数据点向量 **y=y[0]，…，y[99]** ，它是使用已知的 ARIMA 模型生成的。接下来，我们运行自动化模型选择。我们使用推荐的模型来创建下一个数据点**y【100】**的预测 **x** ，然后重复整个过程，改变倒数第五个数据点**y[-5】**。

直观上，我们预计从 **y[-5] - > x** 的映射中会出现一些不连续。时间序列历史的任意微小变化都会导致预测的重大变化。这里有一个例子:

![](img/e073ba8cc17ede77af0dfd5e8452aeb6.png)

请注意，它并不总是不连续的，但是我们经常看到奇怪的反应。跳动的非单调行为并不少见。这里还有一些例子。

![](img/0465164baf5eaa602fd6449337337c04.png)![](img/9c5cd8ed2de531a8c4524f779491cbd4.png)![](img/536ac29889a0c15876326df6a520cc28.png)![](img/536ac29889a0c15876326df6a520cc28.png)![](img/5f2b035ca7e1e955eb50b8ae387bd7b6.png)![](img/2425d0bca0dd2e3c81668659b463d9a0.png)

# 有关系吗？

对于这是否重要的问题，有一些略显不屑的答案，其中一个是“不要担心，因为自动 ARIMA 经常在排行榜上名列前茅(就像[这个](https://microprediction.github.io/timeseries-elo-ratings/html_leaderboards/residual-k_034.html))或者很接近”。嗯，我当然不是建议将历史用于预测的函数的连续性应该是模型选择中唯一的甚至是主要的考虑因素。

另一种反应是假设一个工匠工作流程，在这个流程中有足够的时间来仔细诊断模型顺序选择(比如)并确定一个。你会注意到，我在上面的图中隐含地假设 ARIMA 模型将在每个数据点进行重新调整。这不是绝对必要的，但在现实世界的系列中(例如[这里是](https://www.microprediction.org/browse_streams.html))如果不经常重新校准，模型会很快漂移。

因此，出于这个原因，不太容易回避不连续性的问题。在这里，我认为*是*一个需要解决的问题——特别是如果我们的兴趣是对大量测量量进行纯粹自主的预测。毕竟，连续性的初步证据是由测量误差提供的。如果数据来自一个不完善的测量源，那么仅凭常识就能决定连续性(数学也是如此，只是对误差分布有一些假设)。

然而，连续性并不是我们所熟知和喜爱的预测模型的特征。正如上面的图的标题所示，它们是使用 SkTime 的自动 ARIMA 实现生成的。我正在使用我自己的 [timemachines](https://github.com/microprediction/timemachines) 包来调用——请随意跟踪到 [sk_autoarima](https://github.com/microprediction/timemachines/blob/main/timemachines/skaters/sk/skautoarima.py) 以了解更多细节(是的，这是一个包装上的包装，可以说是一个包装上的包装)。

对于单变量时间序列，我更喜欢 StatsForecast 包，因为它更快。尽管不出所料，我们也会在那里看到同样的不连续性:

![](img/d48b66f162728e71d24704f4131caf09.png)![](img/014efd9181178c0506c3e79a4148b446.png)![](img/0b30a9e8bfca9f026a801e4e916e880f.png)![](img/d35ca31697c68fbf5644fad4e8a3999d.png)![](img/2c33d823577a9f32ca6eb09d2695350a.png)![](img/d9d097e8637d2c9f68b3c95f949ad220.png)

从倒数第五个观察值的敏感度下降到倒数第三个观察值，我们看到了大量不良行为:

![](img/201435fd6555de3b377e7417b8fde00e.png)![](img/a658e83111aee74565c3198dbe9b050a.png)

然而，依靠 ARIMA 模型的结构，人们可能希望对最后一个数据点的敏感性可能表现得更好。这是通常的情况，尽管每个规则都有例外。

![](img/3593de78e4d116a9f0f44f5ed4766517.png)

# 其他流行包装中的不连续性

关于先知生成模型的文章中，有一件事我没有直接提到*[*中的*，那就是它相对于过去观察中的微小变化的不稳定性:](https://towardsdatascience.com/time-series-forecasting-predicting-stock-prices-using-facebooks-prophet-model-9ee1657132b5)*

![](img/108c7256dfafd1bc6b893403721c8d69.png)![](img/79e0fa78aab9d051418190fb0149e8b7.png)![](img/34851018610ca254243b0521837d6b97.png)![](img/e4419e08da2584ab87eda6051faaf143.png)

不过，今天不是痛打先知的时候。我更喜欢的一个模型是 TBATS。它有各种各样的味道。比较靠谱的*bats _ ARMA _ BC*Elo 评分不错(我的标签是 timemachines 包里[代码](https://github.com/microprediction/timemachines/blob/main/timemachines/skaters/bats/allbatsskaters.py)里的滑手名字)。它应用 ARMA 误差和 box-cox。

另一个我称之为 *bats_trendy_arma_bc* 的东西与此类似，但包括趋势识别。两者都是 CPU 密集型的，但是就自主精度而言是强有力的选择，所以观察历史到未来预测的函数映射中存在的不连续性的程度是很有趣的。

![](img/8c82c71e3b47cf1bd77cae06184f2e15.png)

Instability of TBATS with respect to small changes in a past observation

如果您希望了解 Python 生态系统中其他单变量模型的敏感度，timemachines 包很有可能会帮助您实现这一目标。你可以简单的修改运行[skater bump lot . py](https://github.com/microprediction/timemachines/blob/main/examples/sensitivity/skaterbumpplot.py)，假设你能找到你喜欢的包对应的滑手。在[滑冰者](https://github.com/microprediction/timemachines/tree/main/timemachines/skaters)里找找，看看它是否在那里。

# 更平滑的模型

重申一下，我并不提倡抛弃像 Auto-ARIMA 这样优秀的老黄牛，因为有一个特征恰好是这里的研究对象:它们相对于过去观察的规律性。

如果连续性是绝对必须的，那么选择可能相对较少。顺便说一句，我可以给你提供像移动平均的精确加权系综(例子[代码](https://github.com/microprediction/timemachines/blob/main/timemachines/skaters/simple/movingaverage.py))这样的东西，正如你所期望的，表现出轻微的非线性，但不是不连续的。

著名的 Theta 模型是另一个未来预测不应该过于敏感的例子——尽管这个回答多少有些启发性。

![](img/6f7caeb18ea10c9913f8b71597dc6426.png)![](img/3b96cc00a6e5f238550d38480f96532b.png)

另一种方法是继续使用 SARIMA，但避免模型选择——而是使用集成。你将有更多的工作要做，但是如果你坚持 SkTime 的批处理风格，也许你可以尝试一下 [AutoEnsembleForecaster](https://www.sktime.org/en/stable/api_reference/auto_generated/sktime.forecasting.compose.AutoEnsembleForecaster.html) 。然而，我不认为许多合奏会是连续的。

对于极简主义者，或者我们应该说是渐进主义者，timemachines 包中有简洁的[代码](https://github.com/microprediction/timemachines/blob/main/timemachines/skaters/tsa/tsaensembles.py)，直接为您提供各种类型的*在线*基于精度的 TSA 时间序列模型集合(例如)。我建议对 simple/ [movingaverages](https://github.com/microprediction/timemachines/blob/main/timemachines/skaters/simple/movingaverage.py) 中的一些内容进行剪切/粘贴/修改。你可以把其他型号换成这些套装。

timemachines 中的一些[组装](https://github.com/microprediction/timemachines/tree/main/timemachines/skatertools/ensembling)工具在[河](https://github.com/online-ml/river)包中也有相似之处，有些人可能更喜欢后者的字典风格。(river 和 timemachines 的交叉融合被各自的作者认为是一个好主意——尽管时间总是短暂的，拉取请求也是受欢迎的)。

有足够的条件来保持连续性。如果两个组成模型及其权重都是历史的连续函数，那么我们就完成了。然而，对于某些模型来说，前者可能不成立，它们甚至可能不是确定性的。

顺便说一下，权重的连续性也是金融中出现的一个问题，我们在从历史到投资组合权重的映射中考虑规律性或缺乏规律性。这表现为交易成本，因此这是一个严重的问题。但是模型集合和投资组合权重之间的联系是我已经在[优化模型组合](/geekculture/optimizing-a-portfolio-of-models-f1ed432d728b)中讨论过的，所以我不会在这里过多讨论这种可能性，而只是给你指出[经理文档](https://microprediction.github.io/precise/managers.html)。

不要想当然。即使是 TSA 模型的精确加权组合也可能对过去的数据点表现出奇怪的敏感性。

![](img/9f6ec7e6f37025ba47a7264f74e35109.png)

Even a precision-weighted combination of fixed-order TSA models can throw up surprises

# Wiggler:单线在线平滑器

我转向通过强力平滑时间序列模型的*实际*问题——我的意思是将模型应用于多个扩充的历史，并对结果进行平均。实际上，有些人可能认为这不切实际。这是一种懒惰、显而易见且耗时的方式，让从历史到预测的地图变得不那么跳跃。

你知道我不太在意术语。有些人会称之为自举、数据扩充、内核平滑(使用稀疏内核)，甚至是*抖动*。是的，我想我确实记得有一篇论文在时间序列的上下文中使用了“抖动”这个词，尽管如果我现在就能找到它，那就太糟糕了。

速度是一个问题。Python 时序生态系统是 StatsModels 之上的一堆包装器。TSA 在很大程度上——这就是为什么大多数事情都是面向批处理的并且很慢(我在这里提到了一些替代方案)。然而，Nixtla 开发的优秀的自动 ARIMA 包，由于其基于 numba 的性能改进，速度如此之快，以至于批处理方式可能并不总是重要的，即使对于我们现在沉迷的暴力正则化也是如此。

因此，我将坚持使用 [sf_autoarima](https://github.com/microprediction/timemachines/blob/main/timemachines/skaters/sk/sfautoarima.py) 一会儿，因为它的横截面性能(一个好的 [Elo 评级](https://microprediction.github.io/timeseries-elo-ratings/html_leaderboards/univariate-k_002.html))。让我们从开发人员的懒惰中走出来，因为它不需要修改 [StatsForecast.arima](https://github.com/Nixtla/statsforecast/blob/main/statsforecast/arima.py) (尽管如果成功，它可能会建议一些更改，使平滑更有效)。

埋在 [skatertools](https://github.com/microprediction/timemachines/tree/main/timemachines/skatertools) 中的是一个名为 [wiggler](https://github.com/microprediction/timemachines/blob/main/timemachines/skatertools/smoothing/wiggling.py) 的实用程序，它可以修改任何“skater”**f**来创建一个更平滑的时间序列模型。

![](img/5ecca5f82e04f5c3d7febba171f77ac1.png)

它通过维护 3、9、27、81 或更多的 **f** 副本，然后安排一次一个地向它们提供稍微增加的数据点来实现这一点。 **f** 的每个副本的特征在于具有三元条目的元组长度 **m** ，三元条目确定对历史数据点的修改的符号(上/相同/下)。所施加的扰动的大小与时间序列差异的运行估计成比例。

这是一行代码，所以至少你的开发时间被最小化了——如果不是你的挂钟时间。StatsForecast 的 wiggly 版本。已经为您创建了 AutoARIMA 作为示例，如下所示:

![](img/2b76637dd195933d0bd36346745fe5b8.png)

Creating a smoother version of Auto-ARIMA in one line of code

**d** 参数控制数据增加的幅度。你可以在这里用 100 个溜冰者中的任何一个来交换 sf_autoarima。(参见[自述文件](https://github.com/microprediction/timemachines/blob/main/README.md)了解术语“溜冰者”及其签名的解释——我可能应该在之前提到过)。

# 扭动的影响

在下面的图中，红叉表示自动 ARIMA 预报对倒数第三个历史观测值的敏感性。绿色圆圈是摆动的自动 ARIMA 模型的更平滑的响应(通常)。

![](img/8e1118cd07c0d214736e6a0010827688.png)![](img/3ed7678449d19cd03837584de421a093.png)![](img/a90793406c839f03e5fe1680820ea874.png)![](img/167c5454bf096c8a883e6a7a5573f449.png)![](img/49d2b2c7b64ad52fa86fc9063da25cef.png)![](img/ab80f1a42d648f095d00d2bb6e6072c7.png)![](img/9355c264f6a92aef495fcf0105b917f2.png)![](img/49b86bd3511473d3212a1529975fb979.png)

x 轴标签中的“m3”表示原始自动 ARIMA 模型的 27 个副本被平均，每个副本被馈送给它们的数据略有不同，如上所述。一般来说，预测 **x** 对 **y[-3]** 的变化的响应既更平滑又更单调。

不幸的是，绿色模型的计算比红色模型的计算贵 27 倍，但可以说结果是值得的。嘿，看我把这家伙脸上的笑容去掉:

![](img/523b5d5229e7cee98654c1543d59a07d.png)

绿色很吵，但我*认为*这是一种进步。更强有力的预测组合可能会带来更好的结果。使用中间值显然是一种尝试，但是我很可能会添加 Huber means too(从[到](https://github.com/microprediction/precise/blob/main/precise/skaters/locationutil/hubermean.py))等等。

如果你愿意，你可以在 colab 中打开这个[笔记本](https://colab.research.google.com/github/microprediction/timemachines/blob/main/examples/notebooks/wiggling.ipynb)来玩。

# 有成本吗？

到目前为止一切顺利，但是提高规律性的努力会产生反作用吗？我们正在进行的内核平滑的块度使得这种正则化看起来有点笨拙并且容易发生事故——即使这是对计算时间的合理权衡。

顺便说一句，关于意外正规化的话题迫使我告诉你一次，有人跑向收银台，在离开时抢走了一包他们认为是无害的茶。那天晚上晚些时候，我和妻子才惊愕地发现，我们不小心给客人提供了泻药。

![](img/01422f1789d1b1bce1fdba0683e6318b.png)

这是一个真实的故事，我们仍然对此一笑置之。但是平稳地继续，这里需要回答的劳动密集型问题是*摆动器*在提高规律性的努力中是否会无意中损害预测的准确性。我的初步回答有些积极。这似乎不是一个大问题，如果有什么不同的话，它通常会有所帮助。

它不会帮助预测序列 1，2，3，4，5，…但与我之前的评论保持一致，如果数据是带有一些噪声的测量，那么你的额外计算投资，如果你能负担得起，似乎是明智的动机。它可以阻止模型固定在一个 ARIMA 模型的选择，其选择是脆弱的。

同样，你可以在 timemachines repo 中找到各种用于比较性能的工具，我更希望你在自己的问题域中使用它们，而不是依赖我在这里所说的任何东西。当你读到这篇文章的时候，我可能已经添加了 skater tools/[comparison](https://github.com/microprediction/timemachines/tree/main/timemachines/skatertools/comparison)。

碰巧我刚刚开始了一个实验，对我来说，StatsForecast 的波动版本。无论如何，在一组时间序列上，自动 ARIMA 的表现优于原版。因此，至少在这里我们可以吃蛋糕，还可以吃蛋糕(也许再配上泻药茶)。

![](img/a738811c5664c400297b5bf075471718.png)

Highly provisional evidence that a little wiggling does not harm performance

迟早我会对更多“类似 ARIMA”的数据进行更全面的检查——当模型被赤裸裸地使用，而没有处理真实世界测量中的古怪现象的常用技巧时，很难得出结论。与此同时，我鼓励您进行反驳，提出建议，或者尽一切可能添加代码来评估这个粗糙的设备。

# 直到下一次…

我是 GitHub 上的“[微预测](https://github.com/microprediction)”，也是麻省理工学院出版社最近出版的[微预测:建立一个开放的人工智能网络](https://www.amazon.com/Microprediction-Building-Open-AI-Network/dp/0262047322)的作者。我在[英达投资](https://www.intechinvestments.com/)公司工作。