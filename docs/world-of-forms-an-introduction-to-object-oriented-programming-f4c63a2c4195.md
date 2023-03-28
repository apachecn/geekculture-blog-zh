# 表单世界:面向对象编程导论

> 原文：<https://medium.com/geekculture/world-of-forms-an-introduction-to-object-oriented-programming-f4c63a2c4195?source=collection_archive---------24----------------------->

## 抽象、封装、多态和继承

![](img/b836f7b8da539c7990ca081976318a03.png)

By [Gabby K. from Pexels](https://www.pexels.com/photo/crop-faceless-woman-kneading-clay-in-workshop-5302891/).

面向对象程序设计(OOP)自提出以来，已经成为最常用的程序设计范式之一。面向对象方法被广泛实践的部分原因是代码的可重用性。OOP 强调一组打包在一起的概念以及它们与其他对象的关系。这个范例背后的主要思想是*为数据及其相关行为绑定*一个模板。

```
class Object:
        private:
            int int_data;
            string str_data;
        public:
            constructor();
            destructor();
            function get_string();
            function get_int();
            function set_str_data(string _arg);
            function set_int_data(int _arg);
```

上面的伪代码展示了 OOP 的一些基本特性。我们可以观察到对象具有内部(`private`)状态，以及它可以在外部(`public`)做的各种行为。OOP 有两大类:基于类的和基于原型的。在本文中，我们将讨论基于类和基于原型的类别之间的区别。然后我们再来讲 OOP 的核心概念。

基于类的 OOP 围绕着这样一个概念:当一个对象与另一个对象结合时，类之间的继承方式就会发生。相反，基于原型的面向对象通过重用它作为原型的现有对象来扩展功能。基于类的面向对象的一个显著例子是 Java，而 JavaScript 是基于原型的面向对象的一个例子。

考虑实现一个苹果的*概念模型*。在基于类的面向对象中，我们必须对水果的一般属性进行分组，因为苹果属于一种水果。然后，我们将这些特性组合成一个 apple 类的模型，其中实现了 apple 类型的通用属性。与基于原型的面向对象形成对比，在面向对象中，我们可以重用我们的水果资源，并通过添加苹果的通用功能将它们转化为苹果的概念模型。

![](img/469e05e26d74a0bace4b355785c90e2d.png)

Fruits by [Lukas from Pexels](https://www.pexels.com/photo/selective-focus-photography-of-apple-fruits-1352243/).

在 Java 中，我们把这种关系写成:

```
abstract class Fruits{
    private String name;
    Fruits(String name){this.name = name;}
    //generic method and properties of fruits that are common to all fruits
}

class Apple extends Fruits{
    private String id;
    Apple(String id){this.id = id;}
    //generic method and properties of apple that are specific to all fruits
}
```

在 JavaScript 中，我们将这种关系写成:

```
function Fruits(name){
    this.name = name;
    //generic properties of fruits that are common to all fruits
}
// adds generic functions common to all fruits
Fruits.prototype = {
    getName: function(){return this.name;}
}

function Apple(id){
    Fruits.call(this, "Apple");
    this.id = id;
}
// adds generic functions common to all apples
Apple.prototype = {
    getID: function(){return this.id;}
}
//Apple inherits the properties and functions of Fruits
Apple.prototype = new Fruits();

// extending properties of Apple specific to Applies
Apples.prototype.constructor = Apples; 
```

因此，基于原型的 OOP 也被称为无类 OOP。

# 面向对象编程的支柱

面向对象编程基于以下概念:

*   包装
*   抽象
*   遗产
*   多态性

我们来分析一下。

封装是将属性和方法绑定到对象内部的过程。这个概念有效地隐藏了某些特定于对象的函数的实现。在 Java 中通过 private 关键字召唤这个概念是有意义的。

抽象是从属于该类别的对象中挑选出一般特征，这有效地形成了共享资源的层次结构。此外，它有效地减少了所有类的核心函数的实现。没有抽象，您的代码库将充满样板代码；这些代码块在多个地方实现，几乎没有变化。样板文件对于开发来说是一个很大的开销，抽象是消除它们的关键。

继承是一个类的属性向另一个类的扩展。想象一下把乐高积木堆起来形成一个新的物体。堆叠在一起的乐高积木代表了编译器如何在基于类的 OOP 中处理继承:它们扩展了彼此的特性。

多态性是指一个函数有多个实现。这意味着基于函数的关键变量或参数签名，函数可以有多种形式。编译语言有两种多态性:重载和重写。例如，在 Java 中，Java 编译器在编译时推断重载函数的类型，而在运行时推断被覆盖的函数。

![](img/15a46e1813341abaad54ff7f5ccd0984.png)

## 注意事项:

出于所有的意图和目的，这个 JavaScript 片段是用来说明如何实现继承的。作者没有记住 ES6 标准，该标准将类作为面向原型系统之上的语法糖。这样做主要是为了方便使用，但是必须注意 JavaScript 仍然保持着它基于原型的系统。对于对此感兴趣的读者，我在 Hackernoon 上找到了一篇有趣的文章:

[](https://hackernoon.com/once-upon-a-time-in-javascript-inheritance-d24m3235) [## Javascript 中的从前:类和继承|黑客正午

### 早在 2015 年，javascript 中的类首次作为 ECMAScript 6 标准的一部分引入。今天，他们感觉就像…

hackernoon.com](https://hackernoon.com/once-upon-a-time-in-javascript-inheritance-d24m3235)