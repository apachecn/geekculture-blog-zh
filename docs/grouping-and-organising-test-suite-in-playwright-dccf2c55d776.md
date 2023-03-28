# 在剧作家中分组和组织测试套件

> 原文：<https://medium.com/geekculture/grouping-and-organising-test-suite-in-playwright-dccf2c55d776?source=collection_archive---------5----------------------->

剧作家没有一些选项，如测试套件，如果你熟悉量角器，我们有一些选项，如配置文件中的套件，它允许我们在命令行中用- suite 选项执行测试。

如何在剧作家中创建类似 Smoke 或 Regression 的测试套件？

剧作家提供了多种选项来组织和分组您的测试

1.  在文件夹中组织剧作家测试
2.  配置剧作家测试套件

## **在文件夹**中组织剧作家测试

这是一个非常简单的选项，您可以创建多个文件夹和子文件夹，您可以相应地放置相关的测试

例如，如果您想要对与主页导航相关的测试进行分组，只需创建多个 spec 文件，并将它们放在一个文件夹中

```
**PlaywrightFramework
-tests
--home**
---home1.spec.ts
---home2.spec.ts
---home3.spec.ts
**--profile**
---profile1.spec.ts
---profile2.spec.ts
---profile3.spec.ts
```

一旦您在文件夹结构中组织了您的测试，您就可以简单地用命令一起运行所有的测试

```
**npx playwright tests tests/home/**
```

上面的命令将执行您的 **tests/home** 文件夹中的所有测试

# 在剧作家中创建测试套件

如果你想从不同的文件夹中挑选一组测试，上面的选项是不合适的，但是通过标记，你可以很容易地执行一组测试，比如 smoke，regression 等等。

举个例子，如果你想在剧作家中创建测试套件 smoke，那么你可以在剧作家测试中添加标签@smoke，如下所示。

```
**//test1.ts**
 test('Navigate to Google **@smoke**', async ({ page }) => {
    //Some Code
  });**//test2.ts**
test('Some test **@smoke @regression**', async ({ page }) => {
   //Some code
  });**//test3.ts**
test('Some test **@smoke @regression @mysuite**', async ({ page }) => {
   //Some code
  });
```

考虑到上面的例子，我已经创建了多个测试，但是我用不同的标签标记了这些测试，比如 smoke、regression 等等。

现在，如果您只想执行冒烟测试，只需使用下面的命令

```
**npx playright test --grep @smoke**
```

上面的命令执行您在您的剧作家测试脚本中标记为 **@smoke** 的所有测试

希望你喜欢这篇文章。

参考:[https://playwright.dev/](https://playwright.dev/)

☕ [**给我买杯咖啡**](https://www.buymeacoffee.com/ganeshhegde)

**如果您需要任何帮助、支持、指导，请联系我**[**LinkedIn**](https://www.linkedin.com/in/ganeshsirsi/)**|**[【https://www.linkedin.com/in/ganeshsirsi】T21](https://www.linkedin.com/in/ganeshsirsi/)