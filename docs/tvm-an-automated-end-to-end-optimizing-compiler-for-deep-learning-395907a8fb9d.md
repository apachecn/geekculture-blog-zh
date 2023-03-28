# TVM:一个面向深度学习的自动化端到端优化编译器

> 原文：<https://medium.com/geekculture/tvm-an-automated-end-to-end-optimizing-compiler-for-deep-learning-395907a8fb9d?source=collection_archive---------19----------------------->

TVM 是一个开源的深度学习编译器堆栈，用于编译从不同框架到 CPU、GPU 或专业加速器的各种深度学习模型。TVM 还支持 Python、Java 等编程语言的运行时绑定。我们将讨论 TVM 论文的优点和缺点，并做简要总结。

**概要:**

TVM 提供了不同层次的优化。当模型被导入时，第一级优化发生在图中。这种优化提供了图形级融合、布局转换和内存管理。随后的优化发生在张量级——代码生成层。TVM 堆栈有许多层；用 python 编写的支持各种框架的输入模块的用户界面层是框架层。Tensorflow，caffe 模型在这一层被转换成 TVM 兼容图。在计算图形优化层，图形表示通过各种途径进行优化，如预计算修剪，其修剪可在编译时计算的图形节点；布局转换途径，其在层之间存在布局不匹配的情况下跨层添加必要的布局转换操作(或节点);以及融合途径，其基于特定规则将多个节点的计算结合为一个。一种新颖的基于学习的成本模型方法自动优化低级程序的硬件特性，以实现快速代码优化。堆栈中的下一层是调度空间和针对低级和硬件特定优化的优化。引入张量表达式语言来构建操作符并提供生成各种优化程序的程序改变原语。TVM 已在嵌入式 CPU、GPU、嵌入式 GPU 和 FPGA 硬件上实现优化，通过硬件特定优化产生最先进的结果。TVM 支持多硬件优化，初始图形级优化(如修剪、融合)对于每个硬件或多或少是相同的，但对于低级硬件，特定于硬件的优化因硬件而异。

**动机:**

在当今世界，深度学习模型现在可以识别图像，处理自然语言。诸如 TensorFlow、PyTorch 之类的当前框架依赖于计算图形中间表示来实现优化，例如自动微分和动态存储器管理。原始 DL 框架的缺点是，它们支持少量的 GPU 设备，并将特定于目标的优化转移到高度工程化和特定于供应商的操作符库中，这些操作符库需要大量的手动调整，并且由于不透明和专业化而无法跨多个后端进行移植。需要一种系统来为不同的硬件后端提供图形和低级硬件特定的操作员级优化。Tensorflow 和 Pytorch 缺乏这种优化，这导致了基于图形编译器的优化，如 TVM，它支持许多硬件，不需要数据科学开发人员手动调整，他/她可以专注于算法角度。

**纸张的强度:**

1.  比顶级机器学习框架(如 TensorFlow)更好的性能，因为 AutoTVM 模块可以根据主机后端自动调整优化。
2.  在 TVM 中，您可以运行自己的调度，让自动调优器为一组设备找到最佳配置。
3.  TVM 使用服务器级 GPU、嵌入式 GPU、嵌入式 CPU 和基于 FPGA 的定制通用加速器上的真实工作负载。实验结果表明，TVM 提供了跨后端的可移植性能，并实现了比由手工优化库支持的现有框架 1.2 倍到 3.8 倍的加速。
4.  TVM 可以通过重新排序来优化流水线并行性。延迟隐藏调度通过虚拟线程并行化程序，以暴露流水线并行性，从而隐藏存储器访问延迟。

**纸的弱点:**

1.  它不支持将计算划分为多个子图的子图划分，它支持 CPU 上的深度学习优化库，但它将它视为没有优化子图划分的常规 BLAS 库，就像在 TensorRT 中可以看到的那样。如果它能够支持这种集成，那么它将使 DNNL 能够优化和执行最兼容的子图。
2.  TVM 首先进行硬件特定的优化，然后使用自动调谐来获得最佳参数设置。自动调谐实际上花费了大量的时间，这是无法及时编译的。这样，Halide/TVM 程序员可以通过反复检查特定参数设置的性能来更新或重新设计调度。
3.  TVM 编译器不完全支持动态模型，这些模型的输入形状甚至模型本身在执行过程中可能会发生变化。TVM 只支持静态模型。
4.  像 tensorflow，cafe pytorch 这样的深度学习框架支持图形神经网络(GCN)，但深度学习编译器只有原始支持，原因可以归咎于 DL 编译器独特的设计，对 GCN 的支持将在未来的版本中提供。

**参考文献**

1.  [https://www . opensourceforu . com/2019/06/the-capabilities-of-tensor-virtual-machine-an-open-deep-learning-compiler-stack/](https://www.opensourceforu.com/2019/06/the-capabilities-of-tensor-virtual-machine-an-open-deep-learning-compiler-stack/)
2.  [https://ucbrise . github . io/cs 294-ai-sys-sp19/assets/lec12/dl-compilers . pdf](https://ucbrise.github.io/cs294-ai-sys-sp19/assets/lectures/lec12/dl-compilers.pdf)
3.  [https://www . ground ai . com/project/the-deep-learning-compiler-a-comprehensive-survey/1](https://www.groundai.com/project/the-deep-learning-compiler-a-comprehensive-survey/1)
4.  [TVM:用于深度学习的自动化端到端优化编译器](https://arxiv.org/abs/1802.04799)