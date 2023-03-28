# Ansible Boot Camp 6—变量

> 原文：<https://medium.com/geekculture/ansible-boot-camp-5-variables-6782313affc3?source=collection_archive---------4----------------------->

## Ansible 新兵训练营系列

![](img/a7a85e400bff185431f9a823bd1c55c5.png)

# 环境变量

在 Ansible 中，环境变量用于为远程主机上的操作设置环境参数。Ansible 允许您以多种方式处理环境变量。

你可以在游戏、方块或任务级别使用`environment`关键字来设置环境变量…