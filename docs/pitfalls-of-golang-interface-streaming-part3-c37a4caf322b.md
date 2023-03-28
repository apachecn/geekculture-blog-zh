# GoLang 接口流向 JSON 的陷阱(第 3 部分)

> 原文：<https://medium.com/geekculture/pitfalls-of-golang-interface-streaming-part3-c37a4caf322b?source=collection_archive---------8----------------------->

![](img/1bb2d4a491e1e9d07d2f19601580615f.png)

Photo by [Deon Black](https://unsplash.com/@deonblack?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在前两节中，我介绍了一种允许从 JSON 流重建接口类型的方法。为了实现这一点，我在父消息中添加了一个新字段来指示流中结构的类型。

```
type StructX struct {
   X           string      `json:"x"`
   MyInterface MyInterface `json:"my_interface"`
}
```

在溪流中变成了

```
var xr struct {
   X               string      `json:"x"`
   MyInterface     MyInterface `json:"my_interface"`
   MyInterfaceType Type        `json:"my_interface_type"`
}
```

请阅读第一部分和第二部分，看看这个结构来自哪里。

[](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part1-1a067c9bb3cd) [## GoLang 接口流向 JSON 的陷阱(第 1 部分)

### 将接口值传输到 JSON 并返回到 Go 的陷阱。走查和解决方案。

mdcfrancis.medium.com](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part1-1a067c9bb3cd) [](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part2-c1b93a2d7a30) [## GoLang 接口流向 JSON 的陷阱(第二部分)

### 在这个简短系列的第一部分中，我介绍了基本的技术。在这一节中，我将谈到反思和问题…

mdcfrancis.medium.com](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part2-c1b93a2d7a30) 

当我们看到 JSON 流中的嵌入式类型时会发生什么？为了保持一致，我将使用第 1 部分中创建的相同类型。

```
[ 
   {"$type":"StructA", "a":4.56},
   {"$type":"StructA", "a":1.23},
   {"$type":"StructB", "b":"lazy"}
]
```

这里我们有一组异构类型，所有这些类型都实现了我们的 MyInterface 接口。

我们天真地希望下面的代码能够工作，但是仔细想想，我们知道它不会工作，并且会抛出一个错误。

```
func main() {
   b := []byte(`[ 
      {"$type":"StructA", "a":4.56},
      {"$type":"StructA", "a":1.23},
      {"$type":"StructB", "b":"lazy"}
   ]`)
   data := make([]MyInterface, 0)

   err := json.Unmarshal(b, &data)
   if err != nil {
      panic(err)
   }
   fmt.Printf("%s", data)
}
```

这和我们之前遇到的错误一样，Go 不知道如何解组到一个接口类型中。

```
panic: json: cannot unmarshal object into Go value of type main.MyInterface
```

好了，我们可以将一个定制的解组方法绑定到 MyInterface 的数组上，没那么快。

```
func (l []MyInterface) UnmarshalJSON( b []byte) error {
   return nil 
}
```

如果您尝试编译，您将得到以下错误

```
.\main.go:104:7: invalid receiver type []MyInterface
```

如果你深究一下，这是因为[]MyInterface 是一个未命名的类型，而 Go 不允许绑定接收者。这很容易解决，

```
type MyList []MyInterface

func (l *MyList) UnmarshalJSON(b []byte) error {
   return nil
}
```

我们可以命名类型，Go 将很乐意允许我们添加方法。注如第 1 部分所述，解组函数的接收者应该是一个指针。这允许我们修改类型，如果我们不这样做，我们就不能分配切片的长度。

> 这种类型命名是 Go-type 系统中最强大的部分之一，在许多情况下可以用来代替复杂的逻辑。在第 1 部分中，我们使用了一个字符串别名来保证流类型的类型安全。我们还可以将其他方法绑定到该类型，例如，实现 stringer 接口以提供自定义输出。

当我们调用 json 时。解组我们必须提供我们的命名类型，这确保我们将调用我们的自定义方法。

```
var data MyList
err := json.Unmarshal(b, &data)
```

在我们的解组方法中，我们现在有一个字节数组来表示 JSON 对象的完整列表。我们可以开始做一些字符串数学，不建议偷看里面，或者我们可以把它分成一片字节。如果你还记得 json。RawMessage，我们以前用它来执行延迟解码，我们可以在这里做同样的事情。

```
func (l *MyList) UnmarshalJSON(b []byte) error {
   var raw []json.RawMessage
   err := json.Unmarshal(b, &raw)
   if err != nil {
      return err
   } 
   return nil
}
```

我们不必验证 JSON，也不必编写解析器来理解每个元素的结尾在哪里。尽管 Go 将它的解析器应用于 JSON，但这也有一个缺点，尽管我不认为我会做得更好，甚至可能更差。那么，现在我们有了这些数据，我们该怎么做呢？嗯，我们知道我们可以使用 json。根据自定义类型解组，如果该类型只有一个$type 字段，那么我们可以从列表中提取对象的类型。

```
type LazyType struct {
   Type Type `json:"$type"`
}
```

我们将在这里使用一个命名类型，但是我们同样可以内联定义它。现在我们可以遍历二进制文件列表。

```
// Allocate an array of MyInterface
*l = make(MyList, len(raw))
var t LazyType
for i, r := range raw {
   // Unmarshal the array first into a type array
   err := json.Unmarshal(r, &t)
   if err != nil {
      return err
   }
}
```

值得看一下注释后的第一行，我们将接收器的大小分配为与我们的字节列表相同的长度。没有指针接收器，我们无法完成这项工作。最后，我们添加在第 2 部分中使用的逻辑来创建接口的实例。我将在这里添加整个解码器。

```
func (l *MyList) UnmarshalJSON(b []byte) error {
   var raw []json.RawMessage
   err := json.Unmarshal(b, &raw)
   if err != nil {
      return err
   }
   // Allocate an array of MyInterface
   *l = make(MyList, len(raw))
   var t LazyType
   for i, r := range raw {
      // Unmarshal the array first into a type array
      err := json.Unmarshal(r, &t)
      if err != nil {
         return err
      }
      // Create an instance of the type
      myInterfaceFunc, ok := lookup[t.Type]
      if !ok {
         return fmt.Errorf("unregistered interface type : %s", t.Type)
      }
      myInterface := myInterfaceFunc.New()
      err = json.Unmarshal(r, myInterface)
      if err != nil {
         return err
      }
      (*l)[i] = myInterface
   }
   return nil
}
```

这就是我们拥有的。我们可以把我们的 JSON 转换成我们的异构结构列表。如果我运行整个，我得到预期的输出。

```
[%!s(*main.StructA=&{4.56}) %!s(*main.StructA=&{1.23}) %!s(*main.StructB=&{lazy})]
```

具有预期有效负载的两个 StructA 和一个 StructB 的切片。就个人而言，我发现以这种方式融入内置功能比开始手工开发我自己的 JSON 解析器更令人满意。最大的缺点是我们需要解析和遍历 JSON 列表三次才能得到结果。

让我知道你对这个解决方案的想法，我很想听听有其他方法来完成这些流任务的人的意见。

完全可运行样本

```
package main

import (
   "encoding/json"
   "fmt"
)

type Type string
type MyInterface interface {
   Type() Type
   New() MyInterface
}

var lookup = make(map[Type]MyInterface)

func Register(iface MyInterface) {
   lookup[iface.Type()] = iface
}

func init() {
   Register(StructA{})
   Register(StructB{})
}

type StructA struct {
   A float64 `json:"a"`
}
type StructB struct {
   B string `json:"b"`
}

func (_ StructA) Type() Type {
   return "StructA"
}

func (_ StructB) Type() Type {
   return "StructB"
}

func (_ StructA) New() MyInterface {
   return &StructA{}
}

func (_ StructB) New() MyInterface {
   return &StructB{}
}

// Check that we have implemented the interface
var _ MyInterface = (*StructA)(nil)
var _ MyInterface = (*StructB)(nil)

type LazyType struct {
   Type Type `json:"$type"`
}

type MyList []MyInterface

func (l *MyList) UnmarshalJSON(b []byte) error {
   var raw []json.RawMessage
   err := json.Unmarshal(b, &raw)
   if err != nil {
      return err
   }
   // Allocate an array of MyInterface
   *l = make(MyList, len(raw))
   var t LazyType
   for i, r := range raw {
      // Unmarshal the array first into a type array
      err := json.Unmarshal(r, &t)
      if err != nil {
         return err
      }
      // Create an instance of the type
      myInterfaceFunc, ok := lookup[t.Type]
      if !ok {
         return fmt.Errorf("unregistered interface type : %s", t.Type)
      }
      myInterface := myInterfaceFunc.New()
      err = json.Unmarshal(r, myInterface)
      if err != nil {
         return err
      }
      (*l)[i] = myInterface
   }
   return nil
}

func main() {
   b := []byte(`[ 
      {"$type":"StructA", "a":4.56},
      {"$type":"StructA", "a":1.23},
      {"$type":"StructB", "b":"lazy"}
   ]`)

   var data MyList
   err := json.Unmarshal(b, &data)
   if err != nil {
      panic(err)
   }
   fmt.Printf("%s", data)
}
```