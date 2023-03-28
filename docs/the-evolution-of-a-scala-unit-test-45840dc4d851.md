# Scala 单元测试的发展

> 原文：<https://medium.com/geekculture/the-evolution-of-a-scala-unit-test-45840dc4d851?source=collection_archive---------12----------------------->

我有一个 GitHub 项目 Processor6502，这是一个学习项目，我的目的不是为了学习，我计划在 YouTube 视频中使用它来教 6502 汇编程序。

它不是一个仿真器或模拟器，它是一个用户界面，显示 6502 指令执行时寄存器发生了什么。

同样地，对于每个有效指令的每个寻址模式需要至少一个测试，并且对于一些多于一个的指令，例如分支指令需要被测试为分支和不分支。

6502 汇编程序中有 56 条指令，有 13 种寻址模式，但并不是所有的寻址模式对每条指令都有效——对某些指令来说，只有一种有效模式，例如 TXS(将 X 传送到堆栈)只有隐含的寻址模式，而 LDA(加载累加器)有 8 种(立即、2 个零页、3 个绝对和 2 个变址)。

操作码(指令)和有效寻址模式的总数是 151，这是没有任何选项的大量测试！

一条指令有两部分寻址模式——如何获取数据和做什么——操作。

考虑到这一点，我们可以将每个测试视为获取数据和执行操作这两个部分。

在大多数情况下，操作的结果将取决于数据的值，因此这似乎是逻辑起点。

但是挂起寻址模式是在指令中定义的，所以数据获取是该过程的一部分，如果不是指令执行的一部分，那么任何测试都是有效的吗？

在我看来没有，但我们可以使用一个指令来测试我们的测试，LDA 是一个很好的候选，有 8 个有效的寻址模式。

LDA 的 8 种模式是

1.立即
2。零页
3。Zeropage，X
4。绝对
5。绝对的，X
6。绝对，Y
7。间接，X
8。间接，Y

操作是用 operad 加载累加器，这样每个测试都可以断言累加器具有正确的值，否则就会失败。

我们有一个测试流程:

1.将累加器设置为已知值。
2。执行指令以加载不同的值。
3。断言累加器已经更新为正确的值。

立即寻址是指操作数值**立即**跟随内存中的指令。

指令十六进制 A9(十进制 169)后跟十六进制 55(十进制 85)应导致累加器设置为 85。

在我们可以“运行”它之前，我们需要设置测试环境。

被测单元是一个 Scala 对象的执行单元，它的任务是执行处理器 PC 寄存器指向的指令。

安装程序必须设置处理器状态，以便 ExecutionUnit 从内存中执行所需的指令。需要设置存储器本身，使得 2 个字节 A9，55 驻留在处理器程序计数器指向的位置。然后我们可以调用 ExecutionUnit 的 Execute 方法来执行指令。

一旦执行完成，我们就可以验证累加器是否已正确设置为十六进制 55，当然不要忘记程序计数器现在应该指向下一条指令所在的位置(如果存在的话)。

当它工作正常时，我们进行了测试，我们知道 LDA 操作工作正常，但只知道立即寻址模式提取正确。

LDA 零页的操作码是十六进制 A5。零页寻址从存储器的第一个 256 字节(零页)加载操作数，因此执行十六进制字节序列 A5，55 应该用地址位置十六进制 55 中的 e 值加载累加器。

对于这个测试，我们需要将内存位置 55 设置为除十六进制 55 以外的值，否则我们怎么知道我们没有执行 LDA 立即指令呢？

当我们研究可能的寻址模式时，我们发现我们需要间接地址(零页内存中的指针)、绝对地址和索引值。

考虑每种模式时，初始设置变得更加复杂。

完成这一过程需要几个月的时间，但结果是测试现在是数据驱动的，所有 56 条指令都通过对 runTestWithData 方法的一次调用进行了测试，传递的元组列表是:

测试数据标识符
指令数据
初始处理器状态
预期结束状态
存储器的任何预期变化

通过开始简单和不断重构，可以实现有效的测试，维护起来简单灵活，但最重要的是有效。

当我向测试方法添加更多的特性时，发现了以前没有发现的问题，因为测试本身失败了。虫子潜伏在你不想看的地方！

单元测试没有通过，代码通过测试可能是因为测试失败。

当由于代码暴露的问题而导致测试失败时，测试通过。

实际的 LDA 测试

```
"Given a valid LDA instruction " should " execute the correct opcode and value" in { runTestWithData(*dataLdaInstructionTest*)}
```

LDA 测试的数据

```
*// LDA load Accumulator* val *dataLdaInstructionTest* = *List*(
 (“LDA 1.0 Immediate”, InsSourceData(0xA9, *InsData*(10, AccValue(100))), AccResData(10), memVoidResult()),
 (“LDA 1.1 Immediate”, InsSourceData(0xA9, *InsData*(0xF0, AccValue(100))), AccSrResData(0xF0, Negative.mask), memVoidResult()),
 (“LDA 1.2 Immediate”, InsSourceData(0xA9, *InsData*(0x00, AccValueWithCarry(100))), AccSrResData(0, Zero.mask | Carry.mask), memVoidResult()),
 (“LDA 2.0 ZeroPage 101 -> 0x06”, InsSourceData(0xA5, *InsData*(101, AccValue(100))), AccResData(6), memVoidResult()),
 (“LDA 3.0 ZeroPage,x”, InsSourceData(0xB5, *InsData*(100, AccIxValue(100, 1))), AccIxResData(6, 1), memVoidResult()),
 (“LDA 4.0 Absolute absTestLocation -> 0x33”, InsSourceData(0xAD, *InsData*(*absTestLocation*, AccValue(0x64))), AccResData(0x33), memVoidResult()),
 (“LDA 5.0 Absolute,x absTestLocation + 6 -> 0x40”, InsSourceData(0xBD, *InsData*(*absTestLocation*, AccIxValue(0x24, 6))), AccIxResData(0x40, 6), memVoidResult()),
 (“LDA 6.0 Absolute,y absTestLocation + 6”, InsSourceData(0xB9, *InsData*(*absTestLocation*, AccIyValue(0x64, 6))), AccIyResData(0x40, 6), memVoidResult()),
 (“LDA 7.0 (Indirect,x) zeroPageData + 7 -> 0xF0”, InsSourceData(0xA1, *InsData*(*zeroPageData*, AccIxValue(0x64, 7))), AccIxSrResData(0xF0, 7, Negative.mask), memVoidResult()),
 (“LDA 8.0 (Indirect),y”, InsSourceData(0xB1, *InsData*(100, AccIyValue(99, 3))), AccIyResData(0x04, 3), memVoidResult())
 )
```