# 我的 jq 小抄

> 原文：<https://medium.com/geekculture/my-jq-cheatsheet-34054df5b650?source=collection_archive---------0----------------------->

## 带 jq 的 kubectl 太强大了

数百个名称空间中的数十种类型和数千个实例使得获取我想要的东西变得非常困难。例如，`who are owners of certain resources?`

`kubectl`的选项和标志，如`label-selector`、`go-templates`、`jsonpath`等。，可以通过筛选和选择提高查询效率。但是，这些选项通常只支持常规操作，并不适用于所有情况。而且不同的选项有不同的语法规则，很难理解和掌握…