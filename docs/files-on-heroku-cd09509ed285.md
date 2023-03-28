# 赫罗库的档案

> 原文：<https://medium.com/geekculture/files-on-heroku-cd09509ed285?source=collection_archive---------4----------------------->

## 克服 Heroku 短暂文件系统的选择。免费的。

## 挑战

Heroku 文件系统是[短暂的](https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem):应用程序可以在文件系统上写入，但是**当 Dyno(应用程序主机)重启时，任何更改都会被丢弃**，这使得该选项只适用于临时数据。

记住 Heroku 实现[循环](https://devcenter.heroku.com/articles/dynos#restarting)也很重要:每次 Dyno 每 24 小时(至少)重启一次，所有本地文件系统修改都被删除。

## 一个解决方案