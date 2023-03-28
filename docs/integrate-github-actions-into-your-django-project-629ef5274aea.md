# 将 GitHub 动作集成到 Django 项目中

> 原文：<https://medium.com/geekculture/integrate-github-actions-into-your-django-project-629ef5274aea?source=collection_archive---------9----------------------->

*如何自动化林挺和测试用例*

![](img/fb743e28ce99c09fb78e2f56614248ec.png)

Photo by [Rubaitul Azad](https://unsplash.com/@rubaitulazad?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

外面有很多工具。特拉维斯·西，詹金斯，你说吧。但是最近我遇到了 Github Actions，至少对我来说，它让我的生活变得超级简单。

考虑在每次“git 推送”后运行测试用例，GitHub Actions 将为您完成这项工作。
考虑运行“flake8”检查是否…