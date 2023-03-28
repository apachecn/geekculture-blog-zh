# 允许在 MacOS 下的 Conda 环境中使用 XGBoost 的多线程

> 原文：<https://medium.com/geekculture/allow-for-the-multi-threads-of-xgboost-within-conda-environments-under-macos-8959babb4599?source=collection_archive---------48----------------------->

在这篇文章中，我将阐述在 MacOS 下的 Conda 环境中允许 Xgboost 多线程的微妙问题。自从陈、田琦和 Carlos Guestrin (2016)的论文《进化》发表以来，【XGBoost 一直备受关注，在学术研究、工业应用和 Kaggle 等主要竞赛中也越来越受欢迎。强烈建议通读其主页上关于基本思想的内容，人们可以发现它相对于其他集成学习算法的优势。总的来说，不同于传统流行的[随机森林](https://en.wikipedia.org/wiki/Random_forest)分别从随机样本子集和随机特征子集生长的**独立**树中生成平均分数，XGBoost 只是试图通过生长**顺序**树来细化预测结果。在训练过程中，可以通过调节早期停止标准和学习速率以及每棵树的复杂性(深度、叶子数量、权重等)来尝试增加迭代(树)数量的正则化。)来控制过度拟合。一般来说，模型训练过程通常需要几十到几百次的重复试验，基本上是为了选出最佳的超参数集和最佳的特征集。因此，速度问题总是很重要。幸运的是，XGBoost 是优化的高速算法，尤其是自动利用多线程(当然，也有对 GPU 的支持)。不过我刚刚发现了一个微妙的问题，MacOS 下安装的 **XGBoost R 包似乎没有采用多线程，只采用了单线程**。我也在这里提出讨论:[https://github.com/dmlc/xgboost/issues/7017](https://github.com/dmlc/xgboost/issues/7017)

# 全系统 R 的官方解决方案

实际上，开发人员很早就意识到了这个问题，然后提出了简单的解决方案。棘手的是要保证`libomp`提前安装在 MacOS 中。请查看[https://xgboost . readthedocs . io/en/latest/build . html # installing-the-development-version-Linux-MAC-OSX](https://xgboost.readthedocs.io/en/latest/build.html#installing-the-development-version-linux-mac-osx)。因此，使用`brew`即可

```
brew install libomp cmake
```

然后按照说明从源代码安装 XGBoost R 包。例如，只需导航到任意临时目录，然后在终端中输入以下内容

```
git clone --recursive [https://github.com/dmlc/xgboost](https://github.com/dmlc/xgboost)
cd xgboost 
git submodule init 
git submodule update 
mkdir build 
cd build 
cmake .. -DR_LIB=ON 
make 
make install
```

现在，我们可以检查 R 中的以下实验，并看到 XGBoost 现在可以支持多线程:

```
# test number of threadsrequire(xgboost)
x <- matrix(rnorm(100 * 10000), 10000, 100)
y <- x %*% rnorm(100) + rnorm(1000)system.time({
  bst <- xgboost(data = x, label = y, nthread = 1, nround = 100, verbose = F)
})#   user  system elapsed
# 19.257   0.111  17.062system.time({
  bst <- xgboost(data = x, label = y, nthread = 4, nround = 100, verbose = F)
})#   user  system elapsed
# 17.632   0.056   4.450
```

# Conda 环境中的解决方案

以上解决方案只是关于在 MacOS (big sur)下为全系统 R 安装 XGBoost R 包，我主要讲在某 **conda 环境**内安装 XGBoost R 包。一般来说，在大多数情况下，python 经常在 conda 环境中使用。另一方面，R 也可以在 **conda 环境**中安装和设置。实际上，在 **conda 环境**中使用 R 有几个好处。

*   隔离:在 conda 环境中，问题总是可以在不影响系统的情况下进行测试，因为整个 conda 环境可以安全地移除。另一方面，R/Python 包也存储在 conda 环境中，没有对环境外的包的潜在依赖性。
*   https://csantill.github.io/RPerformanceWBLAS/加速:R 对速度不太满意的矩阵计算采用默认的 BLAS:英特尔 MKL 库是针对矩阵计算优化的 BLAS/LAPACK。然而，当使用从 R 网站安装的系统级 R 时，链接 MKL 并不是那么简单。目前，conda 环境可以根据依赖关系中的设置为 R 设置 MKL，例如下面的`yml`文件:

```
name: R_4.0_mkl
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.8
  - conda-forge::r-base=4.1.0
  - conda-forge::libblas=3.9.0=9_mkl
```

*   再现性:这是对的，也是错的。如果 R 包全部使用 conda 安装，那么所有 R 包都可以导出为`yml`文件供同事使用。然而，使用传统的`install.packages()`安装通常是首选，尤其是出于编译目的。然而，使用这种传统方式安装的包**不能**包含和显示在 yml 中。

问题是安装在 conda 环境中的 XGBoost R 包是否支持多线程。你也可以参考我在 https://github.com/dmlc/xgboost/issues/7017 的报道。一般来说，用户在 conda 环境中安装 XGBoost R 包有两种方法。一种是进入 R 后使用`install.packages("xgboost")`另一种是激活环境后在终端使用`conda install -c conda-forge r-xgboost`。但是，这两种方法都只能在**单线程可用**的情况下安装 XGBoost R 包，即使已经安装了`libomp`。在这个问题上谷歌帮不上什么忙，所以我就去查编译 make 文件。当激活这样的 R 环境时，只需在 R 控制台中输入以下**:**

```
file.path(R.home("etc"), "Makeconf")
```

可以在 conda 环境中找到 make 配置**文件的路径。只需使用您喜欢的编辑器打开这样的文件，并找到**

```
SHLIB_OPENMP_CFLAGS = -fopenmp
SHLIB_OPENMP_CXXFLAGS = -fopenmp
SHLIB_OPENMP_FFLAGS = -fopenmp
```

，但以下内容为空

```
SHLIB_CFLAGS = 
SHLIB_CXXFLAGS = 
SHLIB_FFLAGS =
```

从我的试验和实验来看，SHLIB_OPENMP_*不是作为 XGBoost R 包编译的有效标志被**而**调用的。同样**也不能**确定其他请求编译的包是否正确调用它们。由于`libomp`安装在系统范围内，并且`llvm-openmp`也自动安装在包含 R 的 conda 环境中，所以总是添加`-fopenmp`标志应该是无害的。因此，只需通过将标志添加到以下三个空行来修改文件:

```
SHLIB_CFLAGS = -fopenmp
SHLIB_CXXFLAGS = -fopenmp
SHLIB_FFLAGS = -fopenmp
```

现在在 conda 环境中尝试 R 中的`install.packages("xgboost")`。请注意，在编译过程中仍会发现以下信息:

```
checking whether OpenMP will work in a package... no
*****************************************************************************************
         OpenMP is unavailable on this Mac OSX system. Training speed may be suboptimal.
         To use all CPU cores for training jobs, you should install OpenMP by runningbrew install libomp
*****************************************************************************************
```

但是，`-fopenmp`在安装过程中也会出现在标志中。安装后，以下实验应表明`OpenMP`正在使用中:

```
r$> require(xgboost)
    x <- matrix(rnorm(100 * 10000), 10000, 100)
    y <- x %*% rnorm(100) + rnorm(1000)system.time({
      bst <- xgboost(data = x, label = y, nthread = 1, nround = 100, verbose = F)
    })
Loading required package: xgboost
   user  system elapsed
 19.429   0.130  17.317r$> system.time({
      bst <- xgboost(data = x, label = y, nthread = 4, nround = 100, verbose = F)
    })
   user  system elapsed
 17.949   0.063   4.538r$> system.time({
      bst <- xgboost(data = x, label = y, nthread = 8, nround = 100, verbose = F)
    })
   user  system elapsed
 27.401   0.094   3.457
```

# 其他问题

## MacOS 下 Conda 环境下的 XGBoost Python 包怎么样

有趣的是，MacOS 下 conda 环境下的 XGBoost Python 包似乎编译正确并且**多线程可用**。一般来说，XGBoost Python 包可以通过

```
conda install -c conda-forge xgboost
```

下面是 XGBoost Python 包的测试。**在一些用于 Python 的 conda 环境中**、`numpy`和`xgboost`被安装:

```
conda install -c conda-forge numpy libblas=3.9.0=9_mkl
conda install -c conda-forge xgboost
```

然后在`ipython`中:

```
In [1]: import numpy as np
   ...: import xgboost as xgb
   ...: import timeit
   ...:
   ...: data = np.random.rand(10000, 100)
   ...: label = np.random.randint(2, size=10000)
   ...: dtrain = xgb.DMatrix(data, label=label)
   ...:
   ...: param_1 = {'objective': 'binary:logistic', 'nthread': 1, 'eval_metric': 'auc'}
   ...:
   ...: param_4 = {'objective': 'binary:logistic', 'nthread': 4, 'eval_metric': 'auc'}
   ...:
   ...: param_8 = {'objective': 'binary:logistic', 'nthread': 8, 'eval_metric': 'auc'}
   ...:
   ...: num_round = 100In [2]: start = timeit.default_timer()
   ...:
   ...: xgb.train(param_1, dtrain, num_round)
   ...:
   ...: stop = timeit.default_timer()
   ...:
   ...: print('Time: ', stop - start)
Time:  16.160123399In [3]: start = timeit.default_timer()
   ...:
   ...: xgb.train(param_4, dtrain, num_round)
   ...:
   ...: stop = timeit.default_timer()
   ...:
   ...: print('Time: ', stop - start)
Time:  4.242956155000002In [4]: start = timeit.default_timer()
   ...:
   ...: xgb.train(param_8, dtrain, num_round)
   ...:
   ...: stop = timeit.default_timer()
   ...:
   ...: print('Time: ', stop - start)
Time:  3.200284463999999
```

## Linux 系统下的 XGBoost Python/R 包怎么样

幸运的是，根据我在哥鲁达 Linux 上的测试，Linux 系统下 conda 环境**中安装的 XGBoost Python/R 包编译正确，**多线程可以使用**。请查看 MacOS 下 R conda 环境的基本信息**

```
r$> sessionInfo()
R version 4.1.0 (2021-05-18)
Platform: x86_64-apple-darwin13.4.0 (64-bit)
Running under: macOS Big Sur 11.3.1Matrix products: default
BLAS/LAPACK: /Users/mm22204/opt/miniconda3/envs/R_4.0_mkl/lib/libmkl_rt.dyliblocale:
[1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   baseother attached packages:
[1] xgboost_1.4.1.1loaded via a namespace (and not attached):
[1] compiler_4.1.0    magrittr_2.0.1    Matrix_1.3-4      grid_4.1.0
[5] data.table_1.14.0 jsonlite_1.7.2    lattice_0.20-44
```

# 摘要

这个帖子我只是分享一下关于 XGBoost 在 MacOS 下对于多线程可用性的编译问题。其实只是 MacOS 下 XGBoost R 包的细微问题，XGBoost Python 包或者 Linux 系统下的问题是**而不是**。近年来，我主要使用 MacOS 作为平衡选择，以利用类 unix 系统的优点，并获得几个工作和生活软件的访问权限，如 Microsoft Office。然而，实际上编译问题有时似乎是 MacOS 特有的，相应 make 文件的修改是不可避免的。如今，通常将不同的环境应用于不同的项目，以确保独立性和可重复性，conda 是数据科学的通常选择，因为它固有地支持 Python 和 R。然而，我们仍然需要小心包是否被正确编译以利用计算资源。希望这能成为你参考的范例。

# 参考

*   陈，t .，& Guestrin，C. (2016 年 8 月)。Xgboost:一个可扩展的树提升系统。第 22 届 acm sigkdd 知识发现和数据挖掘国际会议论文集*(第 785–794 页)。*