# GoLang 接口流向 JSON 的陷阱(第 1 部分)

> 原文：<https://medium.com/geekculture/pitfalls-of-golang-interface-streaming-to-json-part1-1a067c9bb3cd?source=collection_archive---------9----------------------->

![](img/c603059e30f6cd2d2caeefae5d23e383.png)

Photo by [Jachan DeVol](https://unsplash.com/@jachan_devol?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

有时候编写 go 代码并不像你最初想的那么明显。我们来考虑 JSON 和 Go 之间往返的简单练习。请原谅这些代码示例中被忽略的错误，增加的冗长使示例更难阅读。如果你担心错误，你可以在这里阅读我的一些想法

[](https://mdcfrancis.medium.com/go-error-handling-return-if-9c2e68c15df4) [## 执行错误处理，返回 if？

### 一个“简单”的建议，在不破坏 Go 核心行为的同时，减少 Go 错误的麻烦。Go 2.0 提案。

mdcfrancis.medium.com](https://mdcfrancis.medium.com/go-error-handling-return-if-9c2e68c15df4) 

首先，问题是什么？假设我们有一个类型，我们希望将其转换成 json，然后再转换回 Go 结构，使用内置的 JSON 就可以很容易地实现这一点。元帅和 json。解组库函数。

```
type Test struct {
   T float64 `json:"t"`
}
t := Test{T: 3.142}
b, _ := json.Marshal(t)
var newT Test
_ = json.Unmarshal(b, &newT)
if t != newT {
   panic("all is lost")
}
```

在 main 函数中运行上面的代码，它会顺利执行。如果我打印出字节流的字符串值

```
println(string(b))
```

我们得到了预期的结果:{“t”:3.142 }结构可以深度嵌套，都是刚工作(tm)。这些函数的文档在这里

 [## json

### json 包实现了 RFC 7159 中定义的 JSON 的编码和解码。JSON 和 Go 值之间的映射是…

pkg.go.dev](https://pkg.go.dev/encoding/json) 

它写得很好，但并没有直接涵盖我们要研究的案例。流式接口。在我的代码中，我经常遇到这样的例子，我的结构中有一个成员是接口。默认的方法允许我毫无问题地流这个。下面的代码定义了三个结构，创建了一个嵌入了 A 或 B 的 X 类型的实例，并显示了结果。

```
import "encoding/json"

type Type string
type MyInterface interface {
   Type() Type
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

func (_ StructA) Type() Type {
   return "StructA"
}

func (_ StructB) Type() Type {
   return "StructB"
}

// Check that we have implemented the interface
var _ MyInterface = (*StructA)(nil)
var _ MyInterface = (*StructB)(nil)

func main() {
   // Create an instance of each a turn to JSON
   xa := StructX{X: "xyz", MyInterface: StructA{A: 1.23}}
   xb := StructX{X: "xyz", MyInterface: StructB{B: "hello"}}

   xaJSON, _ := json.Marshal(xa)
   xbJSON, _ := json.Marshal(xb)
   println(string(xaJSON))
   println(string(xbJSON))
}
```

那么，当我们运行这个函数时，我们得到了什么，正如你可能已经猜到的:

```
{"x":"xyz","my_interface":{"a":1.23}}
{"x":"xyz","my_interface":{"b":"hello"}}
```

好的，这很好，现在我可以只取其中一个流，并将其转换回 Go，所以抄袭原始示例:

```
var newX StructX
err := json.Unmarshal(xaJSON, &newX)
if err != nil {
   panic(err)
}
```

现在我们运行这个

```
panic: json: cannot unmarshal object into Go struct field StructX.my_interface of type main.MyInterface
```

花 20 秒钟考虑这一点完全有道理，JSON 解组例程如何“知道”将它解流到什么类型？快速浏览一下 go 文档，就会发现有一个叫做自定义解编函数的东西。如果要这样做，可以通过实现解组器接口来覆盖默认行为。

[https://cs . open source . Google/go/go/+/refs/tags/go 1.19:src/encoding/JSON/decode . go；l=119](https://cs.opensource.google/go/go/+/refs/tags/go1.19:src/encoding/json/decode.go;l=119)

这看起来像什么？如果我们知道我们总是会得到一个接口的特定实现，这应该很容易…

```
func (x *StructX) Unmarshal(b []byte) error {
   ...
}
```

我只需要从输入中取出字节流，并把它转换成带有嵌套 structA 的 structX(假设我们只对 A 进行流处理),但是我该怎么做呢？我不能调用 json。在字节数组上解组，因为这只会递归调用 myslef。这就是 json。RawMessage 前来救援。json。RawMessage 有一个非常简单的实现。

[https://cs . open source . Google/go/go/+/refs/tags/go 1.19:src/encoding/JSON/stream . go；l=263](https://cs.opensource.google/go/go/+/refs/tags/go1.19:src/encoding/json/stream.go;l=263)

为了完整起见，我在这里复制了相关部分

```
// [stream.go - Go (opensource.google)](https://cs.opensource.google/go/go/+/refs/tags/go1.19:src/encoding/json/stream.go;l=263)
// RawMessage is a raw encoded JSON value.
// It implements Marshaler and Unmarshaler and can
// be used to delay JSON decoding or precompute a JSON encoding.
type RawMessage []byte

// MarshalJSON returns m as the JSON encoding of m.
func (m RawMessage) MarshalJSON() ([]byte, error) {
   if m == nil {
      return []byte("null"), nil
   }
   return m, nil
}

// UnmarshalJSON sets *m to a copy of data.
func (m *RawMessage) UnmarshalJSON(data []byte) error {
   if m == nil {
      return errors.New("json.RawMessage: UnmarshalJSON on nil pointer")
   }
   *m = append((*m)[0:0], data...)
   return nil
}

var _ Marshaler = (*RawMessage)(nil)
var _ Unmarshaler = (*RawMessage)(nil)
```

所以，它所做的只是复制一份字节流，你为什么要这样做呢？它允许我们将流的解码(或编码)推迟到以后。要使用它，我需要定义一个不同形状的结构，其中包含原始消息。

```
type StructX struct {
   X           string      `json:"x"`
   MyInterface MyInterface `json:"my_interface"`
}

type StructXRAW struct {
   X           string          `json:"x"`
   MyInterface json.RawMessage `json:"my_interface"`
}
```

现在我可以将我的 json 消息解码成新的结构

```
func (x *StructX) UnmarshalJSON(b []byte) error {
   var xr StructXRAW
   err := json.Unmarshal(b, &xr)
   if err != nil {
      return err
   }
   x.X = xr.X
   return nil
}
```

这仍然没有给出我们想要的结果，因为我们现在只填充了 x.X 的值，并且忽略了[]字节(json。RawMessage)在 MyInterface 字段中。

```
func (x *StructX) UnmarshalJSON(b []byte) error {
   var xr StructXRAW
   err := json.Unmarshal(b, &xr)
   if err != nil {
      return err
   }
   x.X = xr.X
   var a StructA
   err = json.Unmarshal(xr.MyInterface, &a)
   if err != nil {
      return err
   }
   x.MyInterface = a
   return nil
}
```

现在，我可以在接口字段中包含 StructA 的 StructX 之间往返。不幸的是，这确实意味着我需要知道适合接口的底层结构的类型，我想我可以做一些模糊匹配，但还是不要做了。那怎么办呢？好吧，我们知道我们可以有一个定制的封送处理例程和一个定制的解组，为什么不把结构的类型写到流中呢？

# 为什么我们使用指针进行解封，而使用基于值的接收器进行封送？

```
func (x StructX) MarshalJSON( ) ([]byte, error ) {
  ...  
}
```

我确实想指出这个签名的一些问题。在解组的情况下，我们使用指针接收器，在编组的情况下，我们使用值接收器。通常建议不要在给定的结构上混合指针和值接收器。Go FAQ 甚至声明你不应该混淆

[](https://go.dev/doc/faq#methods_on_values_or_pointers) [## 常见问题(FAQ)-Go 编程语言

### 在 Go 诞生的时候，仅仅十年前，编程世界与今天不同。生产软件…

go.dev](https://go.dev/doc/faq#methods_on_values_or_pointers) 

而且，如果你正在使用像 JetBrains 的 [GoLand 这样的工具:不仅仅是一个 Go IDE](https://www.jetbrains.com/go/) (我强烈推荐)，它会不断地为此给你一个警告。那么我们为什么要这样定义呢？考虑以下代码:

```
type Foo struct {
   Foo string
}

func (f *Foo) MarshalJSON() ([]byte, error) {
   panic("Custom Unmarshal Called ")
}

func main() {
   f := Foo{
      Foo: "My String",
   }
   _, _ = json.Marshal(f)
}
```

你预计会发生什么？我天真地以为会产生恐慌，但如果你运行代码，你不会得到恐慌。对于非指针值，不调用指针接收器。当您考虑指针的含义时，这是有意义的，您可以修改底层数据，因此为值调用指针接收器将是错误的做法。对于解组例程，我们必须有一个指针接收器，因为我们要修改这个结构。如果我修改代码来传递一个指向封送函数的指针，我会得到预期的死机

```
func main() {
   f := Foo{
      Foo: "My String",
   }
   _, _ = json.Marshal(&f)
}
panic: Custom Unmarshal Called
```

如果我总是有一个指针类型，这应该没问题，但是如果我有一个包含 X 类型值的结构呢？你猜对了，自定义例程没有被调用。所有这些都消失了。我们为 marshal 方法定义了一个基于值的接收器，在这两种情况下，都会调用正确的例程。

```
func (f Foo) MarshalJSON() ([]byte, error) {
   panic("Custom Unmarshal Called ")
}
```

回到我们的自定义例程，当我们定义一个自定义例程时，我们还定义了另一个公共结构。Go 的一个好处是我们可以简单地内联定义它，并稍微清理一下我们的代码。

```
func (x *StructX) UnmarshalJSON(b []byte) error {
   var xr struct {
      X           string          `json:"x"`
      MyInterface json.RawMessage `json:"my_interface"`
   }
   err := json.Unmarshal(b, &xr)
   if err != nil {
      return err
   }
   x.X = xr.X
   var a StructA
   err = json.Unmarshal(xr.MyInterface, &a)
   if err != nil {
      return err
   }
   x.MyInterface = a
   return nil
}
```

这里我们嵌入了特殊类型，并且只在流函数的范围内可用。因此，让我们做同样的编码，我们将在流中存储类型！

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
} {“x”:”xyz”,”my_interface”:{“a”:1.23},”my_interface_type”:”StructA”}
{“x”:”xyz”,”my_interface”:{
```

现在我们只需要改变解码器来使用类型。

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
   var myInterface MyInterface
   if xr.MyInterfaceType == "StructA" {
      myInterface = &StructA{}
   } else {
      myInterface = &StructB{}
   }
   var a StructA
   err = json.Unmarshal(xr.MyInterface, myInterface)
   if err != nil {
      return err
   }
   x.MyInterface = a
   return nil
}
```

这段代码也值得一提，你会注意到，当我将它们传递给 json.Unmashal 时，我们使用了一个接口类型来包含指向底层结构的指针

```
} else {
   myInterface = StructB{}
}
var a StructA
err = json.Unmarshal(xr.MyInterface, myInterface)
```

我得到一个运行时恐慌！

```
panic: json: Unmarshal(non-pointer main.StructB)
```

哎哟，这就是我过去提到的指针接口二元性，如果你不知道，它会咬你一口。

[](https://mdcfrancis.medium.com/why-do-i-write-golang-in-2021-3ab8f2fff31c) [## 为什么我写 2021 年的 GoLang？

### TL；DR——它满足了我高效工作的需要，具有足够的表现力和性能，几乎可以完成我需要的任何任务。

mdcfrancis.medium.com](https://mdcfrancis.medium.com/why-do-i-write-golang-in-2021-3ab8f2fff31c) 

在下一部分中，我将清理函数并添加一个注册方法。我还将解释为什么在这些情况下反射没有增加很多。

可运行代码示例

```
package main

import (
   "encoding/json"
)

type Type string
type MyInterface interface {
   Type() Type
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
   var myInterface MyInterface
   if xr.MyInterfaceType == "StructA" {
      myInterface = &StructA{}
   } else {
      myInterface = StructB{}
   }
   var a StructA
   err = json.Unmarshal(xr.MyInterface, myInterface)
   if err != nil {
      return err
   }
   x.MyInterface = a
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

[GoLang 接口流式传输到 JSON 的陷阱(第二部分)|作者 Michael Francis | 2022 年 8 月| Medium](https://mdcfrancis.medium.com/pitfalls-of-golang-interface-streaming-to-json-part2-c1b93a2d7a30?source=your_stories_page-------------------------------------)