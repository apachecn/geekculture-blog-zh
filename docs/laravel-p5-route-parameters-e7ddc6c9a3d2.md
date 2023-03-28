# Laravel — P5:路线参数

> 原文：<https://medium.com/geekculture/laravel-p5-route-parameters-e7ddc6c9a3d2?source=collection_archive---------5----------------------->

![](img/4418211e0cd0094090a7bfe4ab438aaa.png)

我们在上一篇文章中讨论了基本的路由和视图。我们经常需要通过 URL 参数向路由文件传递参数。如果你熟悉 PHP，你会知道要捕获像`https://example.com/results.php?page=4`这样的参数，我们需要使用`$_GET`全局数组，即`$_GET[‘page’]`。

# 传递单个参数