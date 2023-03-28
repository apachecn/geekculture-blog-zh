# 使用 Redux Sagas 在 React Native 中轮询 API

> 原文：<https://medium.com/geekculture/polling-api-in-react-native-using-redux-sagas-a68bd243fe4?source=collection_archive---------8----------------------->

![](img/5f2d7bced00ddcb4436c3593c59e9890.png)

Photo by [Joanjo Pavon](https://unsplash.com/@joanjo65?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/repeat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[React Saga](https://redux-saga.js.org/) 是 Redux 的副作用管理器。在这篇文章中，我将向您介绍定期轮询 API 响应的过程。在某些情况下，您可能需要每隔几秒/几分钟调用一次 API。您可以使用 React Sagas 来分派一个动作，该动作可以在指定的时间间隔为您调用 API。