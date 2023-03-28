# 在 Xcode 中测试您的命令行工具

> 原文：<https://medium.com/geekculture/test-your-command-line-tool-in-xcode-7800a42eead6?source=collection_archive---------18----------------------->

终端中的手动测试很简单，但是当从 Xcode 运行程序时，您可能会遇到以下挑战之一。

# 挑战

![](img/6edb16a39c6581d2678ed34d7957225b.png)

如果您的命令行工具定义了强制参数，那么您可能会感到困惑，因为默认情况下没有传递任何参数。您可能面临的另一个挑战是，您希望模拟程序是从特定目录启动的。

别担心。在这篇博文中，我将向你展示如何

1.  启动时传递参数
2.  设置自定义工作目录

# 启动时传递参数

![](img/07d3e1526d367446430e26c150c69880.png)

打开“编辑方案”。

![](img/dce5bdc51e9c44eb7322580e0cdc7829.png)

您可以在运行阶段的“arguments”选项卡中指定启动时传递的参数。

![](img/47f63e506ef1f16163837a562f2bda59.png)

# 设置自定义工作目录

默认情况下，`$(BUILD_PRODUCTS_DIR)`被用作工作目录。要改变这一点，请打开“编辑方案”。您可以在运行阶段的“选项”选项卡中指定一个自定义工作目录。

![](img/dff423b913f5abf6045ffe3ff8753662.png)![](img/66efb358d55b202e3ff10d4d87a7e77b.png)

# 演示

*原载于*[*https://blog . eidinger . info*](https://blog.eidinger.info/test-your-command-line-tool-in-xcode)*。*