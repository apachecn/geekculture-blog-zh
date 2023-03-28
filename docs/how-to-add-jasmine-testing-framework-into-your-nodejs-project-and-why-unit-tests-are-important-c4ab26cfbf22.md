# 如何将 Jasmine 测试框架添加到您的 NodeJS 项目中，以及为什么单元测试很重要

> 原文：<https://medium.com/geekculture/how-to-add-jasmine-testing-framework-into-your-nodejs-project-and-why-unit-tests-are-important-c4ab26cfbf22?source=collection_archive---------14----------------------->

![](img/23f4082c67a29f2926b4518812df35ff.png)

# 茉莉是什么？

> " [Jasmine](https://jasmine.github.io/index.html) 是一个用于测试 JavaScript 代码的行为驱动开发框架。它不依赖于任何其他 JavaScript 框架。它不需要 DOM。它有一个清晰、明显的语法，这样你就可以很容易地编写测试。”
> 
> — [茉莉网站](https://jasmine.github.io/index.html)

Jasmine 是一个结合了 TDD(测试驱动开发)和 DDD(领域驱动设计)的框架，这意味着它有能力帮助开发人员在开发之初基于规范创建单元测试，并包含测试面向对象抽象的工具，如自定义匹配器和间谍，它还包含对其他类型的范例有用的功能，如异常测试。

![](img/c590294f35d2ed2a16a5bee06322cfa5.png)

# 为什么我应该在我的项目中添加单元测试？

单元测试允许验证项目中的每一段代码，它们很重要，因为它们保证了一段代码的行为按预期运行，我们项目中的单元测试越多，我们就越确定我们的项目作为一个整体按预期运行。然而，只有有意识地进行单元测试才能保证这一点，测试每段代码的不同行为，对每段代码至少有一个肯定和否定的案例，并覆盖所有的条件分支，使我们的测试在整个项目中保持一致。

单元测试应该越早越好，这使得团队更愿意约束自己，而不是在项目的中间或结束时痛苦地添加它们，因为所有的代码都已经创建好了。

添加单元测试也给了我们减少修复错误和重构工作量的优势，使项目更短，更容易维护，有助于交付高质量的产品。

就我个人而言，我认为单元测试是一种道德义务。作为一名软件工程师，你正在尽最大努力向用户交付高质量的产品，确保它的所有方面都按计划工作，与其他工程学科的测试过程进行类比(你会乘坐未经测试的飞机旅行吗？).

> “没有测试作为安全网，你要么在每个版本中引入大量的回归错误，要么害怕重构。”
> 
> — [克里斯托夫·j·麦克莱伦](https://softwareengineering.stackexchange.com/questions/319846/agile-without-unit-tests/319848#319848)

![](img/8e1f6f960e5439ee050556d3487380ec.png)

# 如何在你的项目中加入 Jasmine？

要安装 Jasmine，您可以使用以下命令:

```
# *Using yarn*
**yarn** add jasmine --dev# *Using npm*
**npm** install --save-dev jasmine
```

我还建议全局安装 Jasmine，以防您需要直接运行测试，为此您可以运行以下命令:

```
# *Using yarn*
**yarn** global add jasmine# *Using npm*
**npm** install -g jasmine
```

## 如何在 package.json 文件内部创建测试命令？

我建议以如下方式在 package.json 文件中创建“test”命令:

```
{
...
 *"*scripts*"*: {
    *"*test*"*: "jasmine --config=jasmine.json"
  },
...
}
```

正如您在上面的代码中看到的，我们引用了一个 jasmine.json 文件，这意味着我们需要在项目的根目录下创建一个文件。

## 如何创建 jasmine.json 文件？

jasmine.json 文件有一些直接影响测试套件运行方式的属性。

```
{
  "spec_dir": ".",
  "spec_files": [
    "**/*[sS]pec.js",
    "!node_modules/**/*"
  ],
  "helpers": [
    "helpers/**/*.js"
  ],
  "stopSpecOnExpectationFailure": false,
  "random": true
}
```

“spec_dir”属性指定了 Jasmine 将要找到单元测试的目录，在本例中我们将使用“.”选择所有项目目录。

“spec_files”属性包含一个正则表达式列表，它将帮助 Jasmine 查找项目中的 spec 文件(包含单元测试的文件)，第一个表达式查找任何带有“. spec.js”或“Spec.js”后缀的文件，如果我们想更改这个后缀，我们可以在这里进行。下一个表达式将从测试命令中删除“node_modules”目录，单元测试通常包含在这些库中。

您可以在这里找到关于其余这些属性[的更多信息。](https://jasmine.github.io/setup/nodejs.html)

## 如何添加一个 Jasmine 单元测试？

要创建 Jasmine 单元测试，您可以遵循下一个例子，其中创建了一个名为“genetic.spec.js”的规范文件。

```
***const*** { generateIndividual } = **require**('./genetic');**describe**('Genetic module', function() {
  **it**('generateIndividuals function is wrong', **function**() {
    **const** size = 8;
    **const** individual = generateIndividual(size);
    **expect**(individual.length).**toBe**(size);
  });
})
```

前面的例子是一个基本的单元测试，它验证在执行“generateIndividual”函数时是否生成了一个包含 8 个元素的列表。

一旦创建了这个实现，我们就可以将函数添加到“genetic.js”文件中(这是在测试的顶部导入的文件)。

```
**function** calculateRandom(start, limit) {
  return Math.floor((Math.random() * (limit + 1)) + start);
}**function** generateIndividual(length) {
  return Array.from({ length: length }, () => {
    return calculateRandom(0, 1)
  });
}***module****.****exports***.generateIndividual = generateIndividual;
```

一旦我们完成了测试并实现了代码，我们就可以运行测试套件了。

## 测试服怎么跑？

要运行测试套件，我们可以运行以下命令:

```
# *Using yarn*
**yarn** test# *Using npm*
**npm** run test
```

这会给我们一个类似这样的结果:

```
$ jasmine --config=jasmine.json
Randomized with seed 31415
Started
.1 spec, 0 failures
Finished in 0.011 seconds
Randomized with seed 31415 (jasmine --random=true --seed=36989)
✨  Done in 1.27s.
```

我们可以添加另一个测试来确保“generateIndividuals”函数在没有参数的情况下正常工作。

```
**const** { generateIndividual } = **require**('./genetic');**describe**('Genetic module', function() {
  **it**('generateIndividuals function is wrong', **function**() {
    **const** size = 8;
    **const** individual = generateIndividual(size);
    **expect**(individual.length).**toBe**(size);
  });it('generateIndividuals function is wrong without params', **function**() {
    **const** individual = generateIndividual();
    **expect**(individual.length).**toBe**(0);
  });
})
```

如果我们再次运行测试，控制台应该会给出类似如下的结果:

```
Randomized with seed 62831
Started
..2 specs, 0 failures
Finished in 0.015 seconds
Randomized with seed 62831 (jasmine --random=true --seed=33968)
✨  Done in 0.51s.
```

现在您有了一个包含单元测试的项目，这是您构建和交付高质量产品的第一步。

# 结论

单元测试值得你花时间，它们提高了你产品的质量，帮助你成为一个更好更全面的工程师，建议你在你所有的项目中使用它们，这绝对是一个好的实践。有些情况下，在某些项目中，您的经理可能会以“节省时间”为借口告诉您不要将单元测试包括在项目中，因为您有两个选择，花时间修复在影响客户和公司信任的生产后变得昂贵的 bug，或者花时间构建您可以在本地运行和验证的测试，无论哪种方式，迟早有人会发现那个讨厌的 bug，并且最好是您而不是您的用户。

> “[……]作为一名软件开发人员，自动化您的重复测试可以极大地改变您的生活。自动化这些测试，你不再需要盲目地遵循点击协议来检查你的软件是否仍然正常工作。自动化你的测试，你可以毫不费力地改变你的代码库。如果你曾经尝试过在没有合适的测试套件的情况下进行大规模重构，我打赌你知道这是多么可怕的经历。如果你一路上不小心弄坏了东西，你怎么知道呢？[…]"
> 
> — [火腿之声](https://martinfowler.com/articles/practical-test-pyramid.html#TheImportanceOftestAutomation)

我希望你喜欢这篇文章，直到下次。