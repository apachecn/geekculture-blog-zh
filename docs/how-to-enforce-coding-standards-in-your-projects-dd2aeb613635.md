# 如何在您的项目中实施编码标准

> 原文：<https://medium.com/geekculture/how-to-enforce-coding-standards-in-your-projects-dd2aeb613635?source=collection_archive---------11----------------------->

![](img/9ffd9e6dfca9a4178b96d3b6980f6298.png)

Photo by [Martin Shreder](https://unsplash.com/@martinshreder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当您开始任何项目时，您都希望确保您遵循了编码标准，但是如果有多个人在同一个项目上工作呢？你不能在代码评审中经历所有的事情。那你是做什么的？

那么，您可以将 Checkstyle 集成到您的项目中。在本文中，我将向您展示如何在您的 maven 项目中添加 Checkstyle 检查，这样，如果有人不遵循标准，构建将会在您的 CI/CD 管道中失败。

要完全集成它，您只需遵循 2 个步骤。

# 第一步:

在您的项目根目录中添加`checkstyle.xml`

有许多可用的 Checkstyle 标准，但我通常使用上面的。

# 第二步:

在您的`pom.xml`中，只需在您的`<build></build>`中添加以下`plugin`

仅此而已。现在当你做`mvn clean install`时，Checkstyle 将执行静态代码检查，如果发现一些违规，那么构建将失败。

您不需要在 CI/CD 管道中做任何其他事情，因为您的管道无论如何都会运行构建阶段。

希望这有所帮助

干杯！！