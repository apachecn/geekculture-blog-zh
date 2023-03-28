# 本地调试 lambdas，无需 SAM、Docker 或任何东西

> 原文：<https://medium.com/geekculture/debug-lambdas-locally-without-sam-docker-or-anythin-eb68761dd9c4?source=collection_archive---------4----------------------->

最近我在 AWS 中做了很多 lambda 编码。在做了大量的 web 服务之后，我真的失去了在本地环境中一行一行调试代码的能力。我经常使用 PyCharm CE，让 PyCharm 调试我的 lambda 代码，对我来说是一个很棒的主意。不管怎样，使用下面的方法，我们也可以在 VS 代码中设置代码。

我在网上找不到合适的教程。所有的话题都是关于使用 SAM cli，我不想用它。

# 先决条件