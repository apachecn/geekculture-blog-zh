# 创建一个简单的 NextJS 用户列表应用程序第 8 部分— Next.js getStaticPaths 和 getStaticProps

> 原文：<https://medium.com/geekculture/create-a-simple-nextjs-users-list-app-part-8-next-js-getstaticpaths-and-getstaticprops-b84d5b108733?source=collection_archive---------1----------------------->

## 通过获取用户 id 预先构建每条路线

告诉 Next.js 哪些路径需要在构建时呈现。我们需要使用 Next.js `getStaticPaths`函数。Next.js 将静态预渲染所有由`getStaticPath`指定的路径。

![](img/9472744f775896d3a93071436fbd4f3c.png)