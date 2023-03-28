# [git]使现有文件夹受 git 控制，并将它们连接到远程存储库。

> 原文：<https://medium.com/geekculture/git-make-existing-folders-git-controlled-and-connect-them-to-remote-repositories-acd5e59a95e3?source=collection_archive---------11----------------------->

本教程解释了如何将一个现有的文件夹置于 git 的控制之下，并将其绑定到一个远程存储库。

当您首先创建了源代码，然后想要将它们注册到 git 时，这是一个有用的方法。

# 步骤 1:将文件夹设为 git 托管。

打开一个终端，导航到目标文件夹，运行 git init 命令将文件夹置于 git 控制之下。

```
cd path_to_dir
git init
```