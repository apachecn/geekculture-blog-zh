# 探索研究中机器学习管道的 DVC

> 原文：<https://medium.com/geekculture/exploring-dvc-for-machine-learning-pipelines-in-research-part-1-3ebc2ca35a18?source=collection_archive---------18----------------------->

![](img/c9e412d13a0c5384063f772d272c6919.png)

**Cover:** [The Robot Men of Bubble City](http://www.isfdb.org/cgi-bin/title.cgi?2553372) by [James B. Settles](http://www.isfdb.org/cgi-bin/ea.cgi?24744) (variant of [*Fantastic Adventures, July 1949*](http://www.isfdb.org/cgi-bin/title.cgi?148260) 1949)

这是我的 dvc 系列教程的第一部分。(正在进行中的工作)

在过去的几个月里，我工作的一部分变成了为我们在 [HelmholtzAI](https://www.helmholtz.ai) 的团队查看不同的工具来管理机器学习工作流。在这个过程中积累了很多材料，因此我决定分享一些过程和我所学到的东西。

这个想法是分享比较过程本身，但也围绕具体的工具实践教程。我将从后者开始，因为它可以独立阅读，总体评估仍在进行中。

让我们从探索 [DVC](https://dvc.org) 开始，它成为了我们最终的候选之一。

> DVC(数据版本控制)基本上是扩展到机器学习(和类似管道)用例的版本控制

我们将深入学习一个教程，它解决了评估框架适用性的不同重要方面。在探索了基础知识之后，我希望能够分享一个真实的应用实例。

你可以在我们的 [GitHub](https://github.com/hzdr/dvc_tutorial_series) 上找到代码并跟随。:)

我决定分享我的一般方法和为什么教程是这样设计的——如果你只对代码感兴趣，可以直接跳到 **DVC 教程部分。**

## **用例**

选择工具的一个重要方面是始终牢记您的用例。我目前在一家研究机构工作，因此我们的用例包括大型科学数据集、定制基础架构(自有数据中心、HPC)以及对可再现性的特殊需求。基于此，工具必须勾选的框是:

***重现性:*** 可验证的结果对于研究来说至关重要。如果结果是通过在任何地方使用 ML 得到的，那么这一部分对于独立的研究者来说也必须是可重复的。一个典型的 ML 项目包含代码、(几个)数据集、各种参数、(几个)模型、进一步的不同实验等等..

***工作流集成:*** 由于该工具可能会被 IT 专业人员和科学家使用，因此它需要尽可能地无创——你不能指望一个拥有一些基本编码技能的科学家将特定于框架的代码从 ML 代码中分离出来。

***可交换后端:*** 我们有自己的数据中心和云基础设施，因此工具必须能够与定制存储解决方案配合使用。

***框架不可知:*** 它不应该关心我们是使用 pytorch、tensorflow 还是自定义库。如果它也是语言不可知的，这将是首选。

***开源:*** 我们认为应该做的事情，并支持。此外，通常更容易适应客户需求。

***其他:*** 我们更喜欢能够根据自己的需求进行调整的解决方案。此外，预算也很重要。

# **教程设置**

当测试多个框架时，需要一个共同的基础。示例应该在不同的测试场景中保持一致，并在必要时进行调整——每个工具都以不同的方式集成到代码中。这是这个想法的大致框架:

```
**Pipeline:** 
- integrate $tool into workflow
- run ml-pipeline with $tool
**Parameters:**
Test logging, altering and retrieving:
- parameter 1 (val0, val1, val2)
- parameter 2 (val0, val1, val2)
**Metrics:**
Test logging, exchanging and retrieving:
- metric 1 .. metric n
**Models:**
Test exchangeability of models
- model 1 (type, parameters, metrics)
- model 2 (type, parameters, metrics)
**Dataset:**
Storing, altering, versioning and retrieving
- small dataset (PoC)
- large dataset
```

# **例子**

为了使用一些(相对)官方的和经过良好测试的东西，我决定利用在另一个工具 [MLflow](https://mlflow.org/) 的教程部分找到的[示例](https://www.mlflow.org/docs/latest/tutorials-and-examples/tutorial.html)，它也是我们的热门候选工具之一。

事实证明，这个例子需要更仔细的检查和调整。因为理解一个人在做什么总是很有趣的，你可以在这里找到一些关于例子本身的附加信息。

我们开始工作吧！

## **DVC 教程**

我目前使用 OSX，用 [conda](https://docs.conda.io/en/latest/) 管理我的环境。你可以在资源库中找到一个非常基本的**environment . yml***，它列出了你需要的额外的包。*

*开始之前，确保在您的工作环境中已经安装了 DVC。*

*有时我会在命令块中添加一个命令输出，以便更好地理解。*

## ***准备***

*转到您选择的项目目录并克隆存储库:*

```
*git clone [git@github.com](mailto:git@github.com):hzdr/dvc_tutorial_series.git*
```

*切换到源目录并初始化 git 和 dvc:*

```
*cd dvc_tutorial_series/srcgit initdvc init*
```

*添加要使用的远程存储(在这种情况下，我们将留在本地机器上):*

```
*mkdir /tmp/dvc_demo_testdvc remote add -d localremote /tmp/dvc_demo_testdvc remote list*
```

> *DVC 将有关添加数据的信息存储在一个名为 wine-quality.csv.dvc 的特殊文件中，这是一个可读格式的小文本文件。我们可以用 Git 将这个文件版本化为源代码，作为原始数据(列在`. gitignore `中)的占位符*

*查看内容:( [bat](https://github.com/sharkdp/bat) 只是我的首选^^)*

```
*bat .gitignorebat wine-quality.csv.dvc*
```

*将数据添加到 dvc，将占位符文件添加到 git:*

```
*dvc add wine-quality.csvgit add wine-quality.csv.dvc*
```

*将我们的示例代码添加到 git 中(健全性检查总是一个好主意):*

```
*git statusgit add .git commit -m “fresh experiment started”*
```

*将数据推送到 dvc(本地)存储器:*

```
*dvc push~Output:
  1 file pushed*
```

*检查输出:*

```
*ls -R /tmp/dvc_demo_test~Output:
  17
  /tmp/dvc_demo_test/17:
  fbffe83c746612cc247b182e9f7278*
```

## ***测试管道***

*执行 [**dvc 运行**](https://dvc.org/doc/command-reference/run) 创建阶段。阶段是管道的独特步骤，可以通过 git 进行跟踪。*

*阶段还将代码连接到其数据输入和输出(类似于 [Snakemake](https://snakemake.readthedocs.io/en/stable/) )。*

*具有所有相关依赖关系和参数的阶段被写入一个名为*DVC . YAML*的特殊[管道文件。](https://dvc.org/doc/user-guide/project-structure/pipelines-files)*

*我将代码分成不同的脚本来演示分段和流水线过程。*

***创建数据集***

```
*dvc run -f -n prepare\-d create_dataset.py \-o assets/data \python create_dataset.py*
```

***-n** 舞台的名称*

***-d** 这个阶段的附属国*

***-o** 结果输出*

***特征化***

```
*dvc run -f -n featurize \-d create_features.py \-d assets/data \-o assets/features \python create_features.py*
```

*..如我们所见，我们现在有两个依赖项，特征创建脚本和上一步的输出。*

***列车型号***

```
*dvc run -f -n train \-d train_model.py \-d assets/features \-o assets/models \-p model_type,random_state,train \python train_model.py*
```

***-p** 使用 *parameters.yaml* 文件中指定的参数*

*快速浏览*参数. yaml:**

```
*model_type: ElasticNet
lr: 0.0041
random_state: 42train:
  epochs: 70
  alpha: 0.01
  l1_rate: 0.5*
```

***评估模型***

```
*dvc run -f -n evaluate \-d evaluate_model.py \-d assets/features \-d assets/models \-p model_type \-M assets/metrics.json \python evaluate_model.py*
```

***-M** 将指标写入指定的输出目的地，在我们的示例中为 mean_squared_error、mean_absolute_error、r2_score*

*现在，让我们跟踪我们的变化:*

```
*git add assets/.gitignore dvc.yaml dvc.lockgit commit -m “added stages to demo_test”*
```

**dvc.yaml* 现在包含了关于我们运行的命令、其依赖项和输出的信息。*

```
*bat dvc.yaml~Output:
stages:
  prepare:
    cmd: python create_dataset.py
    deps:
    - create_dataset.py
    outs:
    - assets/data
  featurize:
    cmd: python create_features.py
    deps:
    - assets/data
    - create_features.py
    outs:
    - assets/features
  train:
    cmd: python train_model.py
    deps:
    - assets/features
    - train_model.py
    params:
    - model_type
    - random_state
    - train
    outs:
    - assets/models
  evaluate:
    cmd: python evaluate_model.py
    deps:
    - assets/features
    - assets/models
    - evaluate_model.py
    params:
    - model_type
    metrics:
    - assets/metrics.json:
        cache: false*
```

*没有必要使用 [**dvc 为 dvc 添加**](https://dvc.org/doc/command-reference/add) 来跟踪阶段输出， [**dvc 运行**](https://dvc.org/doc/command-reference/run#run) 已经解决了这个问题。*

*你只需要运行 [**dvc 推送**](https://dvc.org/doc/command-reference/push) 就可以把它们保存到远程存储。(在我们的例子中，我们寻址我们的本地存储)*

```
*dvc push~Output:
  8 files pushed*
```

*检查 dvc 推送:*

```
*ls -R /tmp/dvc_demo_test*
```

## ***测试参数变化***

*创建阶段和 dvc.yaml 文件的全部目的是为了在发生变化时能够重现和轻松运行管道。*

*当执行 [**dvc 再现**](https://dvc.org/doc/command-reference/repro) 而不改变代码库或参数时:*

*..什么都没发生*

*现在让我们打开 *params.yaml* 并更改一些参数(例如 alpha 从 0.01 到 0.1)并重新运行管道。*

```
*dvc repro~ Output:
 Stage ‘prepare’ didn’t change, skipping
 Stage ‘featurize’ didn’t change, skipping
 Running stage ‘train’:
   > python train_model.py Updating lock file ‘dvc.lock’

 To track the changes with git, run:
  git add dvc.lock Use `dvc push` to send your updates to the remote storage.*
```

*看看我们上次提交和当前运行之间的变化:*

```
*dvc params diff — all~Output:
 Path Param Old New
 params.yaml lr 0.0041 0.0041
 params.yaml model_type ElasticNet ElasticNet
 params.yaml random_state 42 42
 params.yaml train.alpha 0.01 0.1
 params.yaml train.epochs 70 70
 params.yaml train.l1_rate 0.5 0.5*
```

*让我们跟踪我们的变化:*

```
*git add dvc.lock params.yamlgit commit -m “changed alpha parameter”*
```

*为了方便起见，我们可以通过执行[**DVC Dag**](https://dvc.org/doc/command-reference/dag#dag)**:**将阶段打印成图形*

```
*+---------+
         | prepare |
         +---------+
              *
              *
              *
        +-----------+
        | featurize |
        +-----------+
         **        **
       **            *
      *               **
+-------+               *
| train |             **
+-------+            *
         **        **
           **    **
             *  *
        +----------+
        | evaluate |
        +----------+
+----------------------+
| wine-quality.csv.dvc |*
```

*当给定适当的结构时，DVC 自动确定项目的哪些部分需要重新运行。运行及其结果被缓存，以避免不必要的阶段重新运行。(自动化)*

*[*DVC . YAML*](https://dvc.org/doc/user-guide/project-structure/pipelines-files)*和 [*dvc.lock*](https://dvc.org/doc/user-guide/project-structure/pipelines-files#dvclock-file) 文件描述了使用什么数据以及哪些命令将生成流水线结果。这些文件可以通过 git 进行版本控制和共享。(再现性)**

## ****测试指标变化****

**我们目前使用均方误差(RMSE)和平均绝对误差(MAE)作为度量**

**运行评估管道后 *assets/metrics.json* 的内容:**

```
**{“rmse”: 0.10888875839569741, “mae”: 0.08314237592519587}**
```

**现在，让我们将 r2_score (R2)添加到我们的[**evaluate _ model . py**](https://github.com/hzdr/dvc_tutorial_series/blob/master/src/evaluate_model.py)脚本中，并重新运行这些阶段。Dvc 意识到脚本被修改过，只重新运行了一个阶段:**

```
**dvc repro~Output:
 Stage ‘prepare’ didn’t change, skipping
 Stage ‘featurize’ didn’t change, skipping
 Stage ‘train’ didn’t change, skipping
 Running stage ‘evaluate’:
  > python evaluate_model.py

Updating lock file ‘dvc.lock’**
```

**这次， *assets/metrics.json* 看起来是这样的:**

```
**{“rmse”: 0.10888875839569741, “mae”: 0.08314237592519587, “r2”: 0.9829560472003764}**
```

## ****测试数据集变化****

**一开始，我们创建了一个(本地)远程存储，并执行 [**dvc push**](https://dvc.org/doc/command-reference/push#push) 将本地缓存的数据复制到那个存储。**

**让我们检查一下它是否在那里:**

```
**ls -R /tmp/dvc_demo_test~Output:
 /tmp/dvc_demo_test/17:
 fbffe83c746612cc247b182e9f7278 /tmp/dvc_demo_test/1a:
 240023ddb507d979001525a8ec2669
..**
```

**我们在这里可以看到几个哈希，那么哪个是我们实际的数据呢？**

**查看 *wine-quality.csv.dvc* 可以发现:**

```
**- md5: **17**fbffe83c746612cc247b182e9f7278**
```

**因此，第一个条目包含我们的数据。**

**现在让我们测试一下检索的效果..**

**首先，删除缓存和原始数据:**

```
**rm -rf .dvc/cacherm -f wine-quality.csv**
```

**然后，从存储器中获取数据:**

```
**dvc pull~Output:
 A wine-quality.csv
 1 file added and 6 files fetched**
```

**现在让我们通过在 *wine-quality.csv* 的开头添加一个额外的虚拟瓶子来改变数据集..**

```
**“fixed acidity”,”volatile acidity”,”citric acid”,”residual sugar”,”chlorides”,”free sulfur dioxide”,”total sulfur dioxide”,”density”,”pH”,”sulphates”,”alcohol”,”quality”**7.1,0.28,0.33,19.7,0.046,45,170,1.001,3,0.45,8.8,6 #our fake bottle**7,0.27,0.36,20.7,0.045,45,170,1.001,3,0.45,8.8,6**
```

**..并将实际版本添加到存储中:**

```
**dvc add wine-quality.csv~Output:
 100% Add|  ████████████████████████████████████████████████████████████████████████████████████████████████████|1/1 [00:08, 8.27s/file]**
```

**用 git 跟踪变更..**

```
**git add wine-quality.csv.dvcgit commit -m “updated dataset with fake bottle”**
```

**..并将实际版本推送到存储器:**

```
**dvc push~Output:
 1 file pushed**
```

**哎呀，我们刚刚得知我们的假瓶子原来是醋。让我们回滚到数据集的前一个版本。**

**首先，用 [**git log**](https://git-scm.com/docs/git-log) 理智地检查你当前的状态，看看我们当前的头脑在哪里。然后，退一步回到先前的*状态。dvc* 文件:**

```
**git checkout HEAD~1 wine-quality.csv.dvc~Output:
 Updated 1 path from cb50800dvc checkout~Output:
 M wine-quality.csv**
```

**查 wine-quality.csv，我们的假酒瓶没了。:)**

**不要忘记提交您的更改:**

```
**git commit -m “reverting dataset update"**
```

# ****展望****

**这是系列的第一部分，我们学习了 DVC 的一些基本用法。在接下来的部分中(只要我有时间),将会涉及到像 DVC 非常简洁的实验能力、远程存储工作以及在 HPC(高性能计算)环境中使用 DVC 这样的事情。***

**希望这篇文章能对某人有所帮助:)随时欢迎反馈！**

*** *来自未来的提示:随着 it 进入基础设施，工作情况发生了变化，因此本文暂时保持单篇博文。***