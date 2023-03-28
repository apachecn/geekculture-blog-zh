# GoLang 接口流向 JSON 的陷阱(第二部分)

> 原文：<https://medium.com/geekculture/pitfalls-of-golang-interface-streaming-to-json-part2-c1b93a2d7a30?source=collection_archive---------16----------------------->

![](img/47704929473bac0cb5a14a5e8886895a.png)

Photo by [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这个简短系列的第一部分中，我介绍了在 Go 中与 JSON 之间传输接口类型所需的基本技术。之前我承诺过我会清理函数，没有人喜欢在他们的流代码中使用 if 语句，我会谈谈为什么反射没有真正的帮助。我还会试着让你相信不使用反射并不是一件坏事。

如果您还没有阅读第 1 部分，请停下来继续阅读

[](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part1-1a067c9bb3cd) [## GoLang 接口流向 JSON 的陷阱(第 1 部分)

### 将接口值传输到 JSON 并返回到 Go 的陷阱。走查和解决方案。

mdcfrancis.medium.com](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part1-1a067c9bb3cd) 

好的，我假设你已经遵循了这些步骤，现在已经有了两个结构的工作往返。我在第一部分中忽略的一个项目是接口定义中使用的类型的选择。

```
type Type string
type MyInterface interface {
   Type() Type
}
```

我为 string 创建了一个类型别名，并创建了一个需要实现的简单函数来返回这个类型。如果您继续学习，您还会注意到我在 struct 的流版本中使用了这种类型。

```
func (x StructX) MarshalJSON() ([]byte, error) {
   var xr struct {
      X               string      `json:"x"`
      MyInterface     MyInterface `json:"my_interface"`
      MyInterfaceType Type        `json:"my_interface_type"`
   }
   xr.X = x.X
   xr.MyInterface = x.MyInterface
   xr.MyInterfaceType = x.MyInterface.Type()
   return json.Marshal(xr)
}
```

然而在解码器中，我手动列出了这些类型。这显然容易出错，但是如果没有某种形式的查找，这是你能做的最好的事情。所以现在我们添加一个简单的查找映射

```
var lookup = make( map[Type]MyInterface )
```

还有一个函数，我们可以用它来注册我们的类型

```
func Register(iface MyInterface) {
   lookup[iface.Type()] = iface
}
```

这个映射是一个全局映射，因此任何变化都需要被保护，但是正如你将看到的，我们只在模块初始化时改变这个映射，Go 运行时确保 init 调用的顺序是正确的。

[](https://go.dev/doc/effective_go#init) [## 有效的 Go-Go 编程语言

### 围棋是一门新的语言。虽然它借鉴了现有语言的思想，但它有一些不寻常的属性，使它变得有效…

go.dev](https://go.dev/doc/effective_go#init) 

```
func init() {
   Register( StructA{} )
   Register( StructB{} )
}
```

现在，当我们第一次导入一个模块或运行我们的 main 时，init 函数被调用，我们的映射被填充。我们现在可以将解码器重新定义为

```
func (x *StructX) UnmarshalJSON(b []byte) error {
   var xr struct {
      X               string          `json:"x"`
      MyInterface     json.RawMessage `json:"my_interface"`
      MyInterfaceType Type            `json:"my_interface_type"`
   }
   err := json.Unmarshal(b, &xr)
   if err != nil {
      return err
   }
   x.X = xr.X
   myInterface, ok := lookup[xr.MyInterfaceType]
   if !ok {
      return fmt.Errorf("unregistered interface type : %s", xr.MyInterfaceType)
   }
   err = json.Unmarshal(xr.MyInterface, myInterface)

   if err != nil {
      return err
   }
   x.MyInterface = a
   return nil
}
```

这看起来很棒，但不起作用！我们遇到了和以前一样的问题

```
panic: json: Unmarshal(non-pointer main.StructA)
```

指针接口二元性的事情又来了。问题是我们的映射有一个基于值的条目，我们需要一个指针，但是如果我们存储一个指针，那么我们将共享接口的同一个实例。我的头开始旋转在这一点上。有很多方法可以解决这个问题。我们可以依靠反射来创建该类的新实例，我们可以添加另一个方法来返回该类型的新实例，或者我们可以使用 lambda 函数来返回一个新值。也许最简单的方法是给我们的接口添加另一个方法。

```
type Type string
type MyInterface interface {
   Type() Type
   New() MyInterface
}

var lookup = make(map[Type]MyInterface)

func Register(iface MyInterface) {
   lookup[iface.Type()] = iface
}
```

现在我们实现这些函数

```
func (_ StructA) New() MyInterface {
   return &StructA{}
}

func (_ StructB) New() MyInterface {
   return &StructB{}
}
```

并更新解码器

```
func (x *StructX) UnmarshalJSON(b []byte) error {
   var xr struct {
      X               string          `json:"x"`
      MyInterface     json.RawMessage `json:"my_interface"`
      MyInterfaceType Type            `json:"my_interface_type"`
   }
   err := json.Unmarshal(b, &xr)
   if err != nil {
      return err
   }
   x.X = xr.X
   myInterfaceFunc, ok := lookup[xr.MyInterfaceType]
   if !ok {
      return fmt.Errorf("unregistered interface type : %s", xr.MyInterfaceType)
   }
   myInterface := myInterfaceFunc.New()
   err = json.Unmarshal(xr.MyInterface, myInterface)

   if err != nil {
      return err
   }
   x.MyInterface = myInterface
   return nil
}
```

现在一切都正常了，我们可以注册新的流类型并自动构建它们。请注意，您仍然必须小心，因为没有直接的方法来确保接口指向指针或值。你可以用反射来做到这一点，但是这会导致一个更复杂的环境。

你可能会问，为什么不使用反射呢？与 Java 和其他语言中的反射不同，你不能仅仅通过名字来创建一个类型的实例。您必须首先在映射中注册这些类型，以便可以复制这些类型。这就是 golang gob API 需要注册函数的原因。

 [## 凝块

### 包 gob 管理 gob 流——在编码器(发送器)和解码器之间交换的二进制值…

pkg.go.dev](https://pkg.go.dev/encoding/gob) 

利用这种实现方式

[https://cs . open source . Google/go/go/+/refs/tags/go 1.19:src/encoding/gob/type . go；l=836](https://cs.opensource.google/go/go/+/refs/tags/go1.19:src/encoding/gob/type.go;l=836)

因为我们在这里完全控制，所以在接口上使用一个额外的方法会稍微清楚一些。

就这样，我们现在可以往返访问 JSON 的接口，并探索了延迟解码以及如何通过名称构造类型。

完整代码如下

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
type StructX struct {
   X           string      `json:"x"`
   MyInterface MyInterface `json:"my_interface"`
}

type StructXRAW struct {
   X           string          `json:"x"`
   MyInterface json.RawMessage `json:"my_interface"`
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

func (x StructX) MarshalJSON() ([]byte, error) {
   var xr struct {
      X               string      `json:"x"`
      MyInterface     MyInterface `json:"my_interface"`
      MyInterfaceType Type        `json:"my_interface_type"`
   }
   xr.X = x.X
   xr.MyInterface = x.MyInterface
   xr.MyInterfaceType = x.MyInterface.Type()
   return json.Marshal(xr)
}

func (x *StructX) UnmarshalJSON(b []byte) error {
   var xr struct {
      X               string          `json:"x"`
      MyInterface     json.RawMessage `json:"my_interface"`
      MyInterfaceType Type            `json:"my_interface_type"`
   }
   err := json.Unmarshal(b, &xr)
   if err != nil {
      return err
   }
   x.X = xr.X
   myInterfaceFunc, ok := lookup[xr.MyInterfaceType]
   if !ok {
      return fmt.Errorf("unregistered interface type : %s", xr.MyInterfaceType)
   }
   myInterface := myInterfaceFunc.New()
   err = json.Unmarshal(xr.MyInterface, myInterface)

   if err != nil {
      return err
   }
   x.MyInterface = myInterface
   return nil
}

func main() {
   // Create an instance of each a turn to JSON
   xa := StructX{X: "xyz", MyInterface: StructA{A: 1.23}}
   xb := StructX{X: "xyz", MyInterface: StructB{B: "hello"}}

   xaJSON, _ := json.Marshal(xa)
   xbJSON, _ := json.Marshal(xb)
   println(string(xaJSON))
   println(string(xbJSON))

   var newX StructX
   err := json.Unmarshal(xaJSON, &newX)
   if err != nil {
      panic(err)
   }
   err = json.Unmarshal(xbJSON, &newX)
   if err != nil {
      panic(err)
   }
}
```