# 用 TDD 编写一个好的测试

> 原文：<https://medium.com/geekculture/what-is-a-good-or-bad-test-447307ae97fc?source=collection_archive---------28----------------------->

让我们回顾一下测试驱动开发或 TDD 过程。

**红色** -新的测试最初会失败。

**绿色**——我们努力让测试通过。换句话说，我们编写通过测试所需的最少代码/业务逻辑。

**重构**——我们最终优化了我们的代码/业务逻辑，并再次运行测试。

那么我们写测试的时候应该考虑什么呢？

# 规则一

每个测试应该只测试一项功能。这意味着测试方法通常应该有一个单独的断言…