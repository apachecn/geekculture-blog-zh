# 使用 React 测试库测试 React 上下文 API

> 原文：<https://medium.com/geekculture/testing-react-context-apis-with-react-testing-library-bb34fa808b5a?source=collection_archive---------0----------------------->

本文旨在为使用`**react-testing-library**`的 React 应用程序中的上下文提供者编写测试用例提供一个清晰的概念。然而，本文并不涵盖如何或何时在 React 中使用上下文 API。

此外，通过本教程，您应该对编写测试有所了解。不一定使用 React 测试库。

> C ontext 提供了一种通过组件树传递数据的方法，而不必在每一层手动传递属性。
> - [ReactJS 文件](https://reactjs.org/docs/context.html)