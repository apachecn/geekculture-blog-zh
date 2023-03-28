# 用 GPT-NeoX 探索文本生成

> 原文：<https://medium.com/geekculture/exploring-the-text-generation-with-gpt-neox-de4092935948?source=collection_archive---------10----------------------->

![](img/430c07a3900aac257f7aae1ea7d12522.png)

在寻求开源 GPT-3 像 175B 参数模型的过程中，[eleutherai](https://www.eleuther.ai/)一直在做仪器工作，发布了一个又一个模型。最近从他们的魔法盒子里出来的是[GPT-尼奥克斯](https://github.com/EleutherAI/gpt-neox)！

# 什么是 GPT-奈奥斯？

GPT-NeoX 或 GPT-NeoX-20B 模型是一种自回归语言模型。这是一个与 CoreWeave 合作在 Pile 数据集上训练的 200 亿参数模型。

据称，它是最大的公开可用的预训练通用自回归语言模型。

# 我们可以使用它构建什么类型的应用程序？

借助 GPT-NeoX 模型，我们可以构建文本摘要、释义、代码生成、内容编写、文本自动更正、文本自动完成、聊天机器人和文本生成等应用程序。

# 模型可以使用吗？

是的，这种型号可以使用。GPT-NeoX 的 Github 页面上提供了模型权重链接。Github 资源库链接在参考资料中共享。

# 用 GPT NeoX 生成文本

要从 GPT-NeoX 生成文本，我们需要执行一系列步骤。但在此之前，我们需要一个可以加载 GPT-尼欧克斯模型的实例。

根据 GPT-NeoX 模型 Github 知识库，我们将需要至少 2 个 GPU，我们将需要超过 45GB 的 GPU 内存。此外，还需要 30–40GB 的系统 RAM(内存)。

我们可以使用 GPT NeoX 生成三种类型的文本。

*   **无条件文本生成:**在使用 GPT-NeoX 的无条件文本生成中，我们只是从模型中生成文本，而不向模型提供任何输入。
*   **条件文本生成:**与无条件文本生成相反，条件文本生成需要文本的开始。在向模型提供一个起始句子之后，模型将预测下一个标记，以此类推。
*   **交互式文本生成:**交互式文本生成允许通过命令行界面在用户和语言模型之间进行多轮交互。

# 在 GCP 虚拟机实例上设置 GPT-NeoX 的步骤

要在 GCP/AWS 上设置 GPT-NeoX，我们需要一个具有 45GB GPU RAM 和 40GB CPU RAM 的实例。在 AWS 上，我们将需要 **g4dn.12xlarge** 或类似的实例。

**步骤 1:** 安装必要的 ubuntu 依赖项:

```
sudo apt update
sudo apt install python3-pip python3-dev build-essential libssl-dev libffi-dev
sudo python3-setuptools
sudo apt install virtualenv
sudo apt install git
sudo apt install wget
sudo apt install vim
```

**步骤 2:** 设置 Github 资源库:

```
git clone [https://github.com/EleutherAI/gpt-neox.git](https://github.com/EleutherAI/gpt-neox.git)
cd gpt-neox/
virtualenv env_gpt_neox --python=python3
source env_gpt_neox/bin/activate
pip install torch==1.8.2+cu111 torchvision==0.9.2+cu111 torchaudio==0.8.2 -f [https://download.pytorch.org/whl/lts/1.8/torch_lts.html](https://download.pytorch.org/whl/lts/1.8/torch_lts.html)
pip install -r requirements/requirements.txt
python /root/gpt-neox/megatron/fused_kernels/setup.py install
```

在执行第 2 步时，我们在安装 mpi4py 时遇到了一个错误，并通过以下方法解决了该错误:

```
pip install pip --upgrade
sudo apt install libopenmpi-dev
pip install -r requirements/requirements.txt
```

如果您遇到 YAML 错误，请运行以下命令:

```
pip install -U PyYAML
```

**步骤 3:** 下载模型并更改配置:

以下命令将下载 39GB 的数据。

```
wget --cut-dirs=5 -nH -r --no-parent --reject "index.html*" [https://mystic.the-eye.eu/public/AI/models/GPT-NeoX-20B/slim_weights/](https://mystic.the-eye.eu/public/AI/models/GPT-NeoX-20B/slim_weights/) -P 20B_checkpoints
```

将管道并行大小设置为 GPU 数量的一半。/configs/20B.yml "文件。假设您有 4 个 GPU，那么您的管道并行大小将是 2。

使用命令打开配置文件:

```
vim ./configs/20B.yml
```

并将文件中的“管道平行尺寸:4”更改为“管道平行尺寸:2”。

**第四步:**用 GPT-NeoX 生成文本:

要无条件生成文本，请运行以下命令:

```
python ./deepy.py generate.py ./configs/20B.yml
```

对于条件文本生成:

创建一个 prompt.txt 文件，将您的输入放在文件中，用“\n”分隔，然后运行下面的命令。

```
python ./deepy.py generate.py ./configs/20B.yml -i prompt.txt -o sample_outputs.txt
```

输出将在“sample_outputs.txt”文件中生成。

# 样本文本-由 GPT-NeoX 生成

对于无条件文本生成，下面是一些示例输出:

```
1\. Response: {
    "context": "",
    "text": "A systematic review and meta-analysis of the efficacy of endoscopic balloon dilation for anastomotic strictures after esophagectomy.\nEndoscopic balloon dilation (EBD) is widely used to treat anastomotic strictures after esophagectomy; however, there is no consensus on its efficacy. This systematic review and meta-analysis was performed to assess the efficacy of EBD for anastomotic strictures after esophagectomy. A literature search was performed on PubMed, Embase, and the Cochrane Library to identify eligible studies",
    "length": 102,
    "finished": false,
    "message": null,
    "duration_seconds": 9.70399785041809
}2\. Response: {
    "context": "",
    "text": "Aquaman (2018)\n\nAquaman is the DC Comics hero who can command the seas. He is a powerful swimmer and can communicate with sea life.\n\nAquaman has been the king of Atlantis since the day he was born. He is the only child of the King of Atlantis, Tom Curry, and Queen Atlanna. Aquaman\u2019s parents were killed in a mysterious accident when he was a young boy, and Aquaman was raised by his step-mother",
    "length": 102,
    "finished": false,
    "message": null,
    "duration_seconds": 9.708730936050415
}
```

条件文本生成的示例输出:

```
Response:
{
    "context": "Electric cars will",
    "text": " not be able to compete with conventional vehicles without a government-backed $15,000 rebate.\n\nIn fact, he said the government would need to spend hundreds of billions of dollars to encourage people to switch from their gas guzzlers to electric cars.\n\n\"I can't see it happening. It's not economic. What are we doing with the $15,000 rebate? It's not economic. It's not going to happen.\"\n\nMr Joyce said the lack of infrastructure and battery technology meant electric vehicles were not viable for consumers at this stage.\n\nHe said the Government should instead focus on developing more efficient engines, encouraging people to switch to smaller cars and investing in public transport.\n\n\"We need to get out of this mindset that we're going to have to have electric vehicles to reduce emissions,\" he said....",
    "length": 694,
    "finished": true,
    "message": null,
    "duration_seconds": 69.99025082588196
}Note: "We truncated the text response as we just providing the sample to show the response format."
```

# 模型生成令牌所用的时间

我们还记下了模型加载后生成令牌的时间，以检查 GPT-NeoX 模型的速度性能。

具有以下配置的 Google 云平台虚拟机实例用于生成令牌:

```
Instance name: n1-standard-16
16 vCPUs
60GB RAM
4 Nvidia Tesla T4 GPUs
Ubuntu 20.04
```

生成令牌所用的时间:

```
For 100 tokens it takes around 8-12 seconds
For 200 tokens it takes around 20-25 seconds
For 512 tokens it takes around 45-55 seconds
For 1024 tokens it takes around 100-110 seconds
```

希望你喜欢我们在 GPT-奈奥斯上的实验。我们正在进一步努力对 GPT-NeoX 进行微调，并改善该模型的性能/推理时间。

# 参考资料:

【http://eaidata.bmk.sh/data/GPT_NeoX_20B.pdf 链接 GPT-NeoX 论文:

**链接 https://github.com/EleutherAI/gpt-neox**GPT-NeoX Github 回购:[](https://github.com/EleutherAI/gpt-neox)

***原载于 2022 年 4 月 7 日* [***与 GPT 一起探索文字生成——NeoX***](https://www.pragnakalp.com/exploring-the-text-generation-with-gpt-neox)*。***