# 使用 Vue 命名插槽创建多个模板插槽

> 原文：<https://medium.com/geekculture/using-vue-named-slots-to-create-multiple-template-slots-4aca13a74e0a?source=collection_archive---------9----------------------->

Vue 插槽允许您将内容从父组件注入到子组件中。

这是一个最基本的例子，如果我们不给出来自父容器的任何槽内容，那么我们放在`<slot>`中的任何内容都将是回退内容。

```
<template>
  <div>…
```