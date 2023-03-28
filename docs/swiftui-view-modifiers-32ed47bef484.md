# SwiftUI |视图修改器

> 原文：<https://medium.com/geekculture/swiftui-view-modifiers-32ed47bef484?source=collection_archive---------10----------------------->

## 查看修改器的完整指南

![](img/abc0830f7fb6b3b9129d7f6bb9031135.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/designer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 什么是视图修改器

苹果公司定义视图修改器如下

> *"应用于一个视图或另一个视图修改器的修改器，产生原始值的不同版本"*

换句话说，视图修改器更改先前创建的视图或修改器的当前状态。

比如说。取 Text(“你好”)和 Text(“你好”)。前景颜色(。绿色)。Text()是一个已被调用的视图，视图修饰符是 foregroundColor(。绿色)，这将改变文本的颜色。文本(“你好”)和文本(“你好”)。前景颜色(。绿色)显示不同的结果，因为一个有视图修改器，而另一个没有。

# 实现视图修改器

创建符合视图修饰符协议的结构。一旦调用协议，就会出现错误，因为没有主体创建协议中声明的视图。一旦你修复了它，它看起来会像下面这样。

```
struct TextModifier: ViewModifier {

    func body(content: Content) -> some View {

    }

}
```

如果你看一下参数，你会看到内容。这将加载将应用修改器的视图。所以 Text(“Hello”)是 foregroundColor.green 的上下文。

假设我想要一个视图修改器来改变视图的字体和前景颜色。

```
struct TextModifier: ViewModifier {   
    func body(content: Content) -> some View{
         content
           .font(.system(size:22))
           .foregroundColor(.green)
      }      
}
```

# 应用视图修改器

有两种方法可以应用视图修改器。第一种方法是打电话。修饰符，第二种方法是将它扩展到一个视图，这样就可以直接调用它

让我们看一个第一种方式的例子。

```
struct ContentView: View {
  var body: some View {
    Text("Hello, world!")
      .modifier(TextModifier())
  }
}
```

第二种方式看起来像这样。这就是大多数默认视图修改器的创建方式。

```
extension View{ 
    func TextModifierStyle() -> some View{
         return self.modifier(TextModifier()) 
   }
}struct ContentView: View {
  var body: some View {
    Text("Hello, world!")
      .TextModifierStyle()
  }
}
```

结果如下所示

![](img/622daf9a1a3048ce54ae8f79b487e396.png)

资源

*   [https://developer . apple . com/documentation/swift ui/view modifier](https://developer.apple.com/documentation/swiftui/viewmodifier)