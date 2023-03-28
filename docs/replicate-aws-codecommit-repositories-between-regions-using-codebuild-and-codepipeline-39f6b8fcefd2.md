# 使用 CodeBuild 和 CodePipeline 在区域之间复制 AWS CodeCommit 存储库

> 原文：<https://medium.com/geekculture/replicate-aws-codecommit-repositories-between-regions-using-codebuild-and-codepipeline-39f6b8fcefd2?source=collection_archive---------6----------------------->

使用 CodeBuild 和 CodePipeline 跨多个区域设置 AWS CodeCommit 存储库的连续复制。

将代码库从一个 AWS 区域复制到另一个区域是一项常用的 DevOps 任务。本文演示了如何使用 **AWS CodeBuild** 和 **AWS CodePipeline** 在多个 AWS 区域之间建立一个 **AWS CodeCommit** 存储库的连续复制。这种方法对于维护不同地区的 CodeCommit 存储库的备份非常有用。