# 中如何支持 HTTP 请求取消？网络核心

> 原文：<https://medium.com/geekculture/how-to-support-the-http-request-cancelation-in-net-core-4b5561d9605d?source=collection_archive---------10----------------------->

![](img/54e7dc23daedbf26460c4b8992149090.png)

© Davis Sanchez @Pexels

在某些情况下，您的用户会导航到您的应用程序。然后，它将创建一个对您的服务的 API 调用。然后用户决定离开页面，但是请求将在服务器上执行，直到它准备好。因此，您必须能够取消这个请求，这样才不会陷入资源问题

# 使用 CancellationToken 取消发送的请求…