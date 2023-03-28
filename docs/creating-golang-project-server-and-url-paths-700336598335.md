# 创建 GOLang 项目、服务器和 URL 路径

> 原文：<https://medium.com/geekculture/creating-golang-project-server-and-url-paths-700336598335?source=collection_archive---------22----------------------->

近年来，GoLang 已经成为流行的 web 开发后端语言。在这篇文章中，我将告诉你

1.  如何创建 GoLang 项目？
2.  使用 net/http 模块创建服务器、路由和页面。
3.  将大量路径的代码分成 api 文件。

为了练习，你可以在 [**链接**](https://github.com/ruminder-hub/medium/tree/main/golang-http) 中找到示例代码。

# 创建项目

有人会发现创建项目，导入文件很复杂，但一旦你知道它是非常容易的。使用 brew install go 或从 Golang [**网站**](https://golang.org/dl/) **将 go 安装到您的笔记本电脑上。**

为您的项目创建文件夹。

```
mkdir golang-http
cd golang-http
```

## Go.mod 文件

创建 Go.mod 文件。GoLang 使用 go.mod 文件来跟踪所有的依赖项，即你将在应用程序中使用的类似于 package.json 的库。在创建时，我们将路径传递给它，这将是你的模块或应用程序路径，这样，如果你想让其他开发人员使用他们的模块，他们就可以使用它们。大多数时候我们使用 github.com，因为我们的代码会在上面。

```
go mod init github.com/username/golang-http
```

## Main.go

现在创建 main.go 文件，它将包含 main 函数，这是应用程序的起点。任何文件的第一行都应该是**包包名**。

## **创建路线**

为了创建 API 路由，我们将使用 net/http 包。你也可以使用 mux 或者其他内部实现 net/http 接口的库。
要处理请求，您需要创建一个处理程序，然后创建该处理程序运行的路径或路由。把处理程序想象成将为特定路径运行的控制器。

```
func welcome(writer http.ResponseWriter, request *http.Request){
      io.WriteString(writer, "<h1>Welcome to first goLang    
                     application\n</h1>")
}
```

这个方法将返回上面的方法，并将输出作为请求的响应。您可以使用 fmt 等其他库进行输出。

```
func hello(writer http.ResponseWriter, request *http.Request) {
   fmt.Fprintf(writer, "Hello")
   fmt.Fprintf(writer, "<h1>First GoLang Application</h1>")
}
```

如果您想使用 html 标签，请确保首先添加它们，否则输出将是普通文本。

## 服务器创建和向路由添加处理程序

在 main 函数中，我们将编写所有的请求处理程序和服务器初始化，因为我们希望我们的服务器运行 main 函数。

```
func main() {
   http.HandleFunc("/", welcome) 
   http.HandleFunc("/hello", hello)
   log.Fatal(http.ListenAndServe(":3000", nil))
}
```

http 的 ListenAndServer 方法将在给定的端口和处理程序处启动服务器，该端口和处理程序保持为零以便启动。我们已经把它传给了日志。致命的错误发生时，它会打印日志。这里对于地址"/"我们将调用 welcome 方法。http。HandleFunc 告诉所有对“/hello”的请求将由 hello 方法处理。

## API 层创建

随着时间的推移，应用程序的规模会增加，路线也会增加。将所有的路由处理程序添加到主文件中会使调试变得混乱和困难。我们将根据路由分离处理程序，并将所有具有相同基本路由的路由添加到单个文件中。创建文件夹处理程序，并基于路径创建文件，即 apiHandler.go。将所有具有基本 URL“/API”的方法添加到其中。要使用包(包是文件夹)之外的函数，方法名必须以大写字母开头。

```
package handlers
import (
"fmt"
"io"
"net/http"
)
func WelcomeAPI(writer http.ResponseWriter, request *http.Request) {
    io.WriteString(writer, "<h1>Welcome to first goLang
                          application\n</h1>")
}
func HelloAPI(writer http.ResponseWriter, request *http.Request) {
    fmt.Fprintf(writer, "Hello")
    fmt.Fprintf(writer, "<h1>First GoLang Application</h1>")
}
```

为了导入文件，我们将使用创建 go.mod 文件时使用的路径。

```
package handlers
import (
"fmt"
"github.com/username/golang-http/handlers"
)
```

并使用 http。HandleFunc("/api/")，处理程序。WelcomeAPI)将其用于 API 函数。

我希望你知道如何开始与 GoLang 合作。如有任何问题，请给我留言。祝 GoLang 好运。