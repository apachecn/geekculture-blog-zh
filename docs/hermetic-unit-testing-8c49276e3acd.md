# 密封单元测试

> 原文：<https://medium.com/geekculture/hermetic-unit-testing-8c49276e3acd?source=collection_archive---------11----------------------->

![](img/37258156e5bdfa8f3ae764e830228988.png)

Photo by [CDC](https://unsplash.com/@cdc?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编程的很大一部分是编写测试，以确保您的代码按预期工作。在较小的项目中，编写测试可能看起来很麻烦，但是随着功能的增加、模块的重构和团队的成长，它们将显示出它们的价值。大多数软件工程师都熟悉基本的测试类型，例如[单元测试](https://en.wikipedia.org/wiki/Unit_testing)、[集成测试](https://en.wikipedia.org/wiki/Integration_testing)和[验收测试](https://en.wikipedia.org/wiki/Acceptance_testing)。今天我想谈谈保持单元测试的封闭性，以确保它们仍然是单元测试，而不是集成测试。

密封通常与洁净室实验室联系在一起，但在软件领域，它指的是与外界隔绝或不受外界影响的测试。这些影响可能是依赖关系，比如数据库、库、API，或者不是被测试的特定代码集的任何东西。通过设计进行的单元测试应该有一个狭窄的范围，集中于测试系统的单个目标。例如，只测试一个类的单个方法。这种狭窄的范围使得单元测试易于理解和故障排除，并保持快速和可靠。

例如，考虑图 1，它接受来自外部 API 的散列作为参数。这个散列包含一个摄氏温度，这个温度被转换成华氏温度。

```
def to_fahrenheit(api_return)
  (api_return[:temperature] * 9/5) +32
end**Figure 1: Arbitrary Ruby method to coverting Celsius to Fahrenheit**
```

因为这段代码期望从外部 API 返回，所以很自然地认为单元测试应该在将它传递给`to_fahrenheit`之前调用外部 API。尽管这是可行的，但它并不是真正的单元测试，而是由于外部 API 调用而产生的集成测试。外部调用将增加测试的时间和复杂性，这可能会使这种直接的方法变得不可靠。相反，外部 API 的返回应该用[替换](https://en.wikipedia.org/wiki/Method_stub)，如图 2 所示，以保持单元测试的密封。

```
class TestMeme < Minitest::Test
  def setup
    @fake_api_return = {temperature: 100}
  end

  def test_to_fahrenheit
    assert_equal 212, to_fahrenheit(@fake_api_return)
  end
end**Figure 2: Hermetic Unit Test of to_fahrenheit using** [**Ruby minitest**](https://github.com/seattlerb/minitest)
```

这是一个简单的例子，但是在编写单元测试时，你应该记住这个概念。您是否对数据库或库进行了不必要的调用来测试方法？这个测试失败是因为你写的代码还是你使用的服务宕机了？也就是说，单元测试不会覆盖你所有的测试需求，一些集成测试和验收测试可能是必需的。在上面的例子中，应该添加一个集成测试来检查外部 API 的返回，以确保它是您期望的形状。总的来说，你正在编写测试，让它们为你工作，而不是与你作对。