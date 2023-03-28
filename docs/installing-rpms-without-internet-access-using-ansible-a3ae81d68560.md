# 使用 Ansible 在不访问互联网的情况下安装 rpm

> 原文：<https://medium.com/geekculture/installing-rpms-without-internet-access-using-ansible-a3ae81d68560?source=collection_archive---------2----------------------->

![](img/fd0eb849e321d062af064591da0b106d.png)

Photo by [Liam Briese](https://unsplash.com/@liam_1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## RHEL 8 | rpm | local install | Offline | download only | yum | dnf |

我最近不得不安装一个 Oracle 服务器，但是有一个问题；没有网络。更好的是，操作系统是 Red Hat 8 (RHEL 8 ),没有系统管理器订阅。那么你从哪里开始呢？魔术是有过程的。让我们…