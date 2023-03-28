# 测试 React MobX store？赛普拉斯让它变得简单！

> 原文：<https://medium.com/geekculture/testing-mobx-store-with-react-cypress-made-it-easy-abc5b1571320?source=collection_archive---------14----------------------->

![](img/7b6a9c49dd879f0ece05357d50a32663.png)

这篇博文假设你已经[安装了 Cypress](https://on.cypress.io/installing-cypress) ，理解了 react 术语和状态管理机制。现在，如果你是一个不喜欢阅读的人，让我成为你的[海姆达尔](https://marvelcinematicuniverse.fandom.com/wiki/Heimdall)，打开[源代码](https://github.com/abhinaba-ghosh/cypress-react-mobx-component-testing)的入口。

## 可爱的小反应应用程序

没有强大的 ToDoList 应用程序，任何演示都不会成功。别怪我，我有开发商迷信。

该应用相当简单，由基本组件组成——接收用户输入的