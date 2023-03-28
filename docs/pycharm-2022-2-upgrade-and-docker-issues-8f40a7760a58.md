# Pycharm 2022.2 升级和 Docker 问题

> 原文：<https://medium.com/geekculture/pycharm-2022-2-upgrade-and-docker-issues-8f40a7760a58?source=collection_archive---------18----------------------->

我已经安装了最新的 Pycharm 专业版，你猜怎么着？对 Docker 容器的远程调试不起作用。

[https://intellij-support . jetbrains . com/HC/en-us/community/posts/6870884026898-py charm-2022-2-upgrade-and-Docker-issues](https://intellij-support.jetbrains.com/hc/en-us/community/posts/6870884026898-Pycharm-2022-2-upgrade-and-Docker-issues)从 Intellij 可以看到这是回归的证明。这种回归指向了 4 个月前开放的“将调试器附加到 Django 控制台失败，使用 Docker-compose”[https://you track . jetbrains . com/issue/PY-54084/Attaching-debugger-to-Django-console-fails-with-Docker-compose](https://youtrack.jetbrains.com/issue/PY-54084/Attaching-debugger-to-Django-console-fails-with-Docker-compose)。

因此，这是一种变通方法:

> *1。转到帮助|查找操作|注册表*
> 
> *2。禁用* python.use.targets.api
> 
> *3。从头开始重新创建解释器*

[https://intellij-support . jetbrains . com/HC/en-us/community/posts/6870884026898-py charm-2022-2-upgrade-and-Docker-issues](https://intellij-support.jetbrains.com/hc/en-us/community/posts/6870884026898-Pycharm-2022-2-upgrade-and-Docker-issues)

附:在[“在 Docker 容器中使用 GNOME key ring”](https://alex-ber.medium.com/using-gnome-keyring-in-docker-container-2c8a56a894f7)的故事中，我已经简单地介绍了 Pycharm Profesional 和 Docker 的用法。那时候,- entrypoint 有限制——它只能在调试模式下工作。现在，你可以运行你的 docker 镜像，entrypoint 将被执行，而不是已定义的那个。