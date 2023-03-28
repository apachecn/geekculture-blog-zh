# 揭秘 Golang 中的 HTTP 处理程序

> 原文：<https://medium.com/geekculture/demystifying-http-handlers-in-golang-a363e4222756?source=collection_archive---------1----------------------->

![](img/9caff42a69e770c65c0c861a13097574.png)

Photo by [Mathieu Olivares](https://unsplash.com/@techntravel_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近我开始学习 Golang，并使用 GoLang 本身提供的核心包构建 HTTP 服务。也就是说，我玩了一下 [net/http](https://pkg.go.dev/net/http) 包，以及它如何让开发人员轻松地建立快速 http 服务。

net/http 包使用[处理程序](https://pkg.go.dev/net/http#Handler)来处理发送到特定路径的 http 请求。当深入研究这个包时，我注意到包中定义了各种方法和类型，对于希望使用这个包来构建 HTTP 服务的初学者来说，这在开始时可能会有些混乱。本文的目的是对 Golang 中的处理程序结构有一个简单的了解。

最重要的是，有处理程序接口。处理程序接口是一个带有单一方法 ServeHTTP 的接口，它采用 HTTP。响应和一个 http。请求作为输入。

```
type Handler interface {
	ServeHTTP([ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), *[Request](https://pkg.go.dev/net/http#Request))
}
```

这个方法可以说是“响应”给定 http 请求的核心方法。我们可以在自己的 HandlerClass 中实现这个接口，并给出自己的 ServeHTTP 实现。

```
type helloWorldhandler structfunc (h helloWorldHandler) ServeHTTP(w [ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), r *[Request](https://pkg.go.dev/net/http#Request)){ fmt.Fprintf(w,"HeloWorld")}
```

要将给定的处理程序映射到某个路径，我们可以使用`http.Handle()`方法。

```
func Handle(pattern [string](https://pkg.go.dev/builtin#string), handler [Handler](https://pkg.go.dev/net/http#Handler))
```

调用这个函数时，Go 将给定的处理程序注册到给定的路径，并在请求到达该路径时调用处理程序的 ServeHTTP 方法。

```
http.Handle("/helloWorld",helloWorldHandler)
```

在本例中，当 HTTP 请求命中“/helloWorld”路径时，将调用 helloWorldHandler 的 ServeHTTP 方法。

上述处理 HTTP 方法的一个缺点可以说是，如果我们想要处理来自不同路径的大量 HTTP 请求，我们需要为实现处理程序接口的每个单独的处理程序创建处理程序类型。为此，net/http 包提供了一个特殊的函数`HandleFunc`。

```
func HandleFunc(pattern [string](https://pkg.go.dev/builtin#string), handler func([ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), *[Request](https://pkg.go.dev/net/http#Request)))
```

`HandleFunc`的职责与`http.Handle(w,r)`相同，都是向一个处理程序注册一个给定的路径，但是它以不同的方式完成。如果你看一下上面方法的签名，你会发现它将一个函数作为`HandleFunc()`的第二个参数，而不是像`http.Handle()`中的处理程序类型。

所以在这种情况下我们可以给 http。HandleFunc()任何具有签名`func([ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), *[Request](https://pkg.go.dev/net/http#Request))`的函数，它将使用该函数来处理命中给定路径的请求。因此，这里不需要为每条路径创建处理程序类型。

```
myHelloWorldFunc := func (w [ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), r *[Request](https://pkg.go.dev/net/http#Request)) 
{fmt.Fprintf(w,"HeloWorld Function")}http.HandleFunc("/helloWorld", myHelloWorldFunc)
```

最后是类型 [HandlerFunc](https://pkg.go.dev/net/http#HandlerFunc) 。这可以说是一个适配器，它允许我们像 http 一样使用普通的功能。汉德勒。

如果你看看这个类型的定义，

```
type HandlerFunc func([ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), *[Request](https://pkg.go.dev/net/http#Request))
```

您可以看到 HandlerFunc 是一个具有类型`func([ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), *[Request](https://pkg.go.dev/net/http#Request))`的类型。HandlerFunc 类型本身实现 ServeHTTP 函数，如下所示。

```
type [HandlerFunc](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;bpv=1;bpt=1;l=2045?gsn=HandlerFunc&gs=kythe%3A%2F%2Fgolang.org%3Flang%3Dgo%3Fpath%3Dnet%2Fhttp%23type%2520HandlerFunc) func([ResponseWriter](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;drc=refs%2Ftags%2Fgo1.16.6;l=95), *[Request](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/request.go;drc=refs%2Ftags%2Fgo1.16.6;l=103))func ([f](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;bpv=1;bpt=1;l=2048?gsn=f&gs=kythe%3A%2F%2Fgolang.org%3Flang%3Dgo%3Fpath%3Dnet%2Fhttp%23param%2520HandlerFunc.ServeHTTP%253Af) [HandlerFunc](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;drc=refs%2Ftags%2Fgo1.16.6;l=2042)) [ServeHTTP](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;bpv=1;bpt=1;l=2048?gsn=ServeHTTP&gs=kythe%3A%2F%2Fgolang.org%3Flang%3Dgo%3Fpath%3Dnet%2Fhttp%23method%2520HandlerFunc.ServeHTTP)([w](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;bpv=1;bpt=1;l=2048?gsn=w&gs=kythe%3A%2F%2Fgolang.org%3Flang%3Dgo%3Fpath%3Dnet%2Fhttp%23param%2520HandlerFunc.ServeHTTP%253Aw) [ResponseWriter](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;drc=refs%2Ftags%2Fgo1.16.6;l=95), [r](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/server.go;bpv=1;bpt=1;l=2048?gsn=r&gs=kythe%3A%2F%2Fgolang.org%3Flang%3Dgo%3Fpath%3Dnet%2Fhttp%23param%2520HandlerFunc.ServeHTTP%253Ar) *[Request](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/net/http/request.go;drc=refs%2Ftags%2Fgo1.16.6;l=103)) { f(w,r)}
```

所以我们可以说 HandlerFunc 函数本身实现了 http。处理程序接口。如上所示，如果我们查看 ServeHTTP 方法的实现，我们可以看到，它所做的只是调用 HandlerFunc 所基于的主函数。如果我们看一个例子，

```
myHelloWorldFunc2 := func (w [ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), r *[Request](https://pkg.go.dev/net/http#Request)) 
{fmt.Fprintf(w,"HeloWorld Function")}handlerFromHelloFunc := http.HandlerFunc(myHelloWorldFunc2)http.Handle("/helloWorld", handlerFromHelloFunc)
```

所以在上面的例子中我们可以看到，函数 myHelloWorldFunc2 被`http.HandlerFunc()`转换为`http.Handler`类型，然后在`http.Handle`方法中作为普通的处理程序使用。

这篇简短的文章到此结束，文章的目的是让读者对使用 core net/http 包处理 HTTP 请求的各种实现有一点了解。作为总结，我们讨论了处理 HTTP 请求的给定路径的 3 种主要方法。

1.  创建一个处理程序类型，实现 ServeHTTP 方法并使用`http.Handle(path, Handler)`注册它。
2.  用签名 `func (w [ResponseWriter](https://pkg.go.dev/net/http#ResponseWriter), r *[Request](https://pkg.go.dev/net/http#Request))`创建一个函数并使用`http.HandleFunc(path, function)`注册它
3.  使用`http.HandlerFunc`支持类型创建一个函数并将其转换为处理程序，并在`http.Handle(path, handler)`方法中使用它来注册它。

如果您读到本文的这一部分，我希望您已经对 Golang 中不同的 HTTP 处理程序有了更多的了解。如果您有任何问题或更正，请随时评论。

关注更多关于 API、微服务、Java 和 Golang 的内容:)

[](https://www.linkedin.com/in/rashmin95/) [## Ravindu Rashmin -软件工程师- WSO2 | LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Ravindu Rashmin 的个人资料。Ravindu 列出了 7 个工作…

www.linkedin.com](https://www.linkedin.com/in/rashmin95/) [](https://github.com/rashm1n) [## rashm1n -概述

### 喜欢 java，音乐和足球块或报告一个基本的 Java 项目创建一个 AWS Lambda 函数 Java 后端…

github.com](https://github.com/rashm1n)