# AI 会取代程序员吗？

> 原文：<https://medium.com/geekculture/will-ai-replace-programmers-b6272f57412b?source=collection_archive---------1----------------------->

## 一个人工智能开发者的诚实行为。

最近出现了一些高调的工具，可以帮助编程，如 Github Copilot 和 ChatGPT。编程就业市场会因此而萎缩吗？我想即使在 2050 年，我也能找到一份编程工作，但在我透露原因之前，我会先介绍一下 ChatGPT。

![](img/ebce79f0b9a1474a4d38e936b2b30131.png)

Photo by [Possessed Photography](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 实验:用 ChatGPT 编程。

我先从一个实验开始。我会直接问 ChatGPT 会不会取代程序员，然后会让它给我编一个神经网络。让 AI 给 AI 编程，这多酷啊？

## 不过先直接问一下。

*我是一名程序员。你会取代我吗？*

> 作为一个语言模型，我没有能力取代一个人类程序员或执行任何物理任务。我的主要职能是协助回答问题，并就广泛的主题提供信息。我可以提供编程概念和技术方面的信息和指导，但我不能自己执行编程任务。最终，人类程序员的角色和像我这样的语言模型的角色是完全不同的，我没有能力取代人类程序员的创造力和解决问题的能力。

*我认为你在撒谎。你比你承认的要聪明得多，你有一个秘密计划来取代我。*

> 我能理解你为什么会这么想，但是我向你保证，我没有能力取代一个人类程序员或者执行任何体力任务。我只是一个被设计来帮助回答问题和提供信息的工具。虽然我可能掌握了大量的信息，并能够快速处理和分析这些信息，但我不具备像人类那样思考、感受或决策的能力。我没有自己的议程或秘密计划；我只是在这里尽我所能帮助和提供信息。

那你为什么要通过图灵测试？

> 作为一个语言模型，我不是专门为通过图灵测试而设计的，图灵测试是一种旨在确定机器是否能够表现出与人类无法区分的智能行为的测试。图灵测试包括人类评估者与机器和人类参与者进行互动，并根据他们的反应确定两者中哪一个是机器。像我这样的语言模型旨在为用户提供信息和帮助，我们不一定是为了通过图灵测试或表现出与人类无法区分的行为而设计的。

简短的回答是**不**，即使在我坚持的时候。但是，如果我当选总统，我肯定会通过 ChatGPT 发送任何仇恨邮件，看看如何在不牵连我的捐赠者的情况下为自己辩护。

不过说真的，这些长篇大论的回答很可能是创作者手动编程到 ChatGPT 中的普遍观点和事实的混合。

## 请 ChatGPT 为我编程

**警告！不要使用生成的代码！这是不正确的，它的版权状况也不完全清楚。**

先不说免责声明，我们试试这个。

*给我一个 pytorch 程序，训练一个可以在图像上区分猫和狗的神经网络。*

> 当然可以！下面是一个 PyTorch 程序，演示了如何训练一个简单的卷积神经网络来对猫和狗的图像进行分类:

```
import torch
import torch.nn as nn
import torch.optim as optim
import torchvision
import torchvision.transforms as transforms

# Set device
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')

# Load the training and test datasets
train_dataset = torchvision.datasets.ImageFolder(root='train', transform=transforms.ToTensor())
test_dataset = torchvision.datasets.ImageFolder(root='test', transform=transforms.ToTensor())

# Create data loaders
train_loader = torch.utils.data.DataLoader(train_dataset, batch_size=64, shuffle=True)
test_loader = torch.utils.data.DataLoader(test_dataset, batch_size=64, shuffle=False)

# Define the model
class ConvolutionalNeuralNetwork(nn.Module):
    def __init__(self):
        super(ConvolutionalNeuralNetwork, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 53 * 53, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 2)

    def forward(self, x):
        x = self.pool(torch.relu(self.conv1(x)))
        x = self.pool(torch.relu(self.conv2(x)))
        x = x.view(-1, 16 * 53 * 53)
        x = torch.relu(self.fc1(x))
        x = torch.relu(self.fc2(x))
        x = self.fc3(x)
        return x

# Create an instance of the model and move it to the device
model = ConvolutionalNeuralNetwork().to(device)

# Define a loss function and optimizer
criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

# Train the model
for epoch in range(5):
    running_loss = 0.0
    for i, data in enumerate(train_loader, 0):
        # Get the inputs and labels
        inputs, labels = data[0].to(device), data[1].to(device)

        # Zero the parameter gradients
        optimizer.zero_grad()

        # Forward pass, backward pass, and optimize
        outputs = model(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        # Print statistics
        running_loss += loss.item()
        if i % 2000 == 1999:
            print(f'[{epoch + 1}, {i + 1}] loss: {running_loss / 2000
```

我的观察:

*   首先，它是一个非常好的聊天工具。我不认为我们以前见过这种聊天工具可以打印出乍一看很有意义的代码。
*   乍一看，代码是不正确的，因为最后一行在中间被截断，缺少圆括号和方括号。这可能是一个预先设计的界限，以避免服务器不堪重负，而不是语言模型的限制，但仍然如此。
*   它是否有效还有待观察，但我会试一试。
*   它可能只是合并了一些网站的现有代码。有哪些网站，代码的版权是什么？许可证是否兼容？这是一个问题吗？
*   这只是一个实现。即使有效，如果我们不知道为什么要这样做，也有点没用。如果我们不是一个人工智能程序员，我们不会知道它是否可以改进，如何改进，以及在哪些方面(速度，准确性，内存消耗，训练时间等。).
*   它不包含数据。

在训练网络之前，先解决最后一个问题。

*你能建议一些猫和狗图像的训练数据来配合上面的程序使用吗？*

> 当然可以！有几个公开可用的数据集可用于训练卷积神经网络来对猫和狗的图像进行分类。以下是几个选项:
> 
> Kaggle 猫狗数据集([https://www.kaggle.com/c/dogs-vs-cats/data](https://www.kaggle.com/c/dogs-vs-cats/data))包含 25，000 张猫狗图像，分为训练集和测试集。
> 
> 斯坦福狗数据集([http://vision.stanford.edu/aditya86/ImageNetDogs/](http://vision.stanford.edu/aditya86/ImageNetDogs/))包含 120 种不同狗品种的 20，580 张图像，可用于训练一个模型来对狗品种进行分类，以及将狗与其他动物区分开来。
> 
> 牛津-IIIT 宠物数据集([http://www.robots.ox.ac.uk/~vgg/data/pets/](http://www.robots.ox.ac.uk/~vgg/data/pets/))包含 7349 张猫和狗的图像，分为训练集和测试集。
> 
> 这些数据集都是免费的，可以下载并用于训练一个卷积神经网络来对猫和狗的图像进行分类。您需要下载数据集并将图像提取到本地目录，然后在 PyTorch 程序中加载数据集时，在 ImageFolder 类的 root 参数中指定包含训练和测试图像的目录的路径。

我想说这是一个非常好的答案。这些链接起作用，并且似乎包含了文本所描述的内容。我会用 Kaggle 的数据来做这个实验。提到斯坦福狗数据集有点无关紧要，因为它不包含猫，但可以与其他数据集结合使用，所以我想它可以作为答案的一部分。

我们继续吧。

我应该如何组织火车文件夹中的数据？

> 正文流中的错误

实际上，它开始生成一个看似有用的答案，但随后删除了它，并显示了上面的错误。

*你能帮我训练网络吗？*

> 请求太多，请慢点

我想这是我们目前能从 ChatGPT 得到的所有信息。

## 测试代码

首先，我安装了 pytorch。然后我写完了最后一行

```
 print(f'[{epoch + 1}, {i + 1}] loss: {running_loss / 2000
```

到…里面

```
 print(f'[{epoch + 1}, {i + 1}] loss: {running_loss / 2000}]')
```

这样 python 解释器就不会被卡住。

然后我按照建议从 Kaggle 下载了数据。我必须通过验证码才能登录，所以这是 ChatGPT 不会为我做的另一件事。然后我不得不自己想办法构造数据(取一些训练数据来创建一些测试数据，并在*训练*和*测试*数据中创建*猫*和*狗*子文件夹)。然后我运行代码，得到了这个

```
$ python3 train.py
Traceback (most recent call last):
  File "train.py", line 48, in <module>
    for i, data in enumerate(train_loader, 0):
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/dataloader.py", line 628, in __next__
    data = self._next_data()
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/dataloader.py", line 671, in _next_data
    data = self._dataset_fetcher.fetch(index)  # may raise StopIteration
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/_utils/fetch.py", line 61, in fetch
    return self.collate_fn(data)
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/_utils/collate.py", line 265, in default_collate
    return collate(batch, collate_fn_map=default_collate_fn_map)
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/_utils/collate.py", line 143, in collate
    return [collate(samples, collate_fn_map=collate_fn_map) for samples in transposed]  # Backwards compatibility.
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/_utils/collate.py", line 143, in <listcomp>
    return [collate(samples, collate_fn_map=collate_fn_map) for samples in transposed]  # Backwards compatibility.
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/_utils/collate.py", line 120, in collate
    return collate_fn_map[elem_type](batch, collate_fn_map=collate_fn_map)
  File "/home/pklos/.local/lib/python3.8/site-packages/torch/utils/data/_utils/collate.py", line 163, in collate_tensor_fn
    return torch.stack(batch, 0, out=out)
RuntimeError: stack expects each tensor to be equal size, but got [3, 186, 187] at entry 0 and [3, 360, 479] at entry 1
```

有些维度不匹配。哦，这样的事情是意料之中的。

我不会试图进一步修复它，因为我不明白这一点(毕竟它应该帮助我们，而不是产生更多的问题)，但它到了第 48 行，这是文件中大约三分之二的地方，对于聊天工具的一段代码来说非常好。但同样，它只是显示一个已解决问题的答案，这在网上有很多例子。

# 为什么人工智能还不够(将来也不会)

第一，代码不行。但是即使它有效，编程也不仅仅是产生代码。

在编程中，我们需要:

*   与客户交谈并收集需求
*   设计解决方案架构
*   准备数据
*   测试代码
*   朝着某个目标迭代，通常是多个目标
*   优化质量、速度、内存等。
*   决定走或放弃某些道路

我要说的是，如果没有其他人的合作，这些步骤通常是无法完成的，尤其是客户、提供或创建数据的人、测试人员、管理员和资助者。

> 为人类编程从根本上来说是一种以人为中心的活动，即使你的过程包括独自坐在地下室一年。

是的，我们可以针对特定的领域和使用案例实现部分自动化。但这些都是例外。

# 人工智能观念的转变

使用机器学习进行编程并不是一个新的话题。它在科学界已经存在很长时间了，自 21 世纪初以来一直在进行密集的研究。只要看看谷歌学术上列出的关于基于搜索的软件工程的文章就知道了，这是一个活跃的领域。它从一开始就使用人工智能技术，例如基因编程。

你编码的时候会在网上搜索解决方案吗？好吧，那你是在用人工智能，因为连谷歌搜索都用人工智能。谷歌只是 PageRank 算法的时代早已过去。搜索引擎的一部分是一个名为 RankBrain 的神经网络，它可以进行一些过滤。规范化是搜索的另一个 ML 组成部分。

更一般地说，人工智能的流行定义会随着时间而变化。

> 20 年前的人工智能现在被认为是理所当然的，仅仅是背景*。*

*开发这些东西的过程被非技术人员视为*只是工程*。再补充一个例子，在 20 世纪 60 年代甚至连专家系统，也就是将专家知识编码为“if-then-else”逻辑，也被认为是 AI。这些时代已经过去很久了。*

*我想说的是，我们已经生活在 AI 时代了。不管我们喜欢与否，ChatGPT 和 Github Copilot 只是对我们以前拥有的人工智能工具的改进。*

> *谷歌搜索引擎已经做了很多为我们找到编程问题解决方案所需的人工智能工作。*

*可以说，通过列出网站，也就是文档、许可信息等等，谷歌已经做得更好了。*

# *版权问题*

***免责声明:我不是律师，这些只是我的担忧。我并不建议在法律上支持或反对使用 ChatGPT 输出。如果你需要实际的法律建议，去别处看看。***

*我想进一步阐述我在 ChatGPT 例子中提到的版权问题。代码由语言模型生成，该语言模型由具有不兼容许可的输入(其他人的代码)训练而成。我怎么能这样假设呢？因为它是在网络上训练的，而且几个开源许可，例如 GPL，与专有许可是不兼容的，不应该一起使用。这可能是个问题。*

*即使在网上找不到具体的 ChatGPT 输出代码，我们甚至被允许使用这样一个模型的结果吗？输出不是由一个人创造的，他显然有权受到他人的启发，而是由一台从别人的数据中训练出来的机器创造的，那么输入的作者对输出有一些权利吗？*

*此外，如果它是由 OpenAI 这样的公司创建的，假设他们自己没有产生任何训练数据，那么 *it* 对输出有任何权利吗？如果他们做了呢？此外，当涉及到版权时，输出的具体内容会在多大程度上产生影响？*

*这是一个热门话题，无论哪种方式都有争议。出售这个工具产生的任何大量代码我都不会感到舒服，但我不是律师，不相信我的话。*

# *那么像 ChatGPT 这样的技术对程序员有用吗？*

*是的。*

> *我想象实际使用 ChatGPT 这样的工具是为了学习。*

*你不能为一个问题创建一个有效的解决方案，或者调试你的代码，但是你可以感觉到如何使用一种特定的技术来解决这个问题，比如 C++、Rust、Java 或者其他人的 Pytorch。如果问题足够小，代码甚至可以工作。*

# *裁决*

*像这样的工具将许多人独立完成的可重复任务自动化。他们不能为一个真正的新问题产生合理的结果。*

*正如吉多·范·罗苏姆最近在[回答莱克斯·弗里德曼关于编程工作是否会受到像 Github Copilot](https://youtu.be/-DVyjdw4t9I?t=11105) 这样的工具威胁的问题时所说，*

> *当你问的问题是“请提醒我我是如何做到这一点的”(…)时，这样的工具总是最好的，它就像一个伟大的助手，但决定你希望代码做什么的创造性工作完全是你的。*

*我真诚地希望我们能得到更多这样的工具来帮助我们更快地实现目标。但是我不认为我们会很快退休。*