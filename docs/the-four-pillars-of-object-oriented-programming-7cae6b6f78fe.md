# 面向对象编程的四大支柱

> 原文：<https://medium.com/geekculture/the-four-pillars-of-object-oriented-programming-7cae6b6f78fe?source=collection_archive---------27----------------------->

![](img/2bee5cbad830cd12fe05a5e48825e4b7.png)

Photo by [Duncan Kidd](https://unsplash.com/@we_the_royal?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

面向对象编程(OOP)是一种基于“对象”思想的编程范式。编程范式可以被认为是程序员在编写代码以帮助完成某项任务时遵循的一组规则。OOP 的四个支柱是那些规则集。这些支柱代表了帮助程序员编写干净代码的设计原则。

这四大支柱是:

*   抽象
*   包装
*   遗产
*   多态性

今天，我将逐一谈论这些原则。让我们来看看。

# 抽象

抽象就是隐藏一个流程的实现细节。想想做披萨的过程。要完成这个过程，您必须采取几个步骤。其中一些步骤可能是

*   擀面团
*   用调味汁覆盖比萨饼面团
*   磨碎奶酪
*   用奶酪覆盖比萨饼
*   预热烤箱
*   把比萨饼放进烤箱

现在想象一下，如果你不得不一天为多人做多次比萨饼。如果我们能按下一个按钮来完成整个过程，不是更好吗？这就是抽象的意义所在。如果我们在代码中考虑它，我们可以定义一个包含所有指令的函数，然后我们可以重用这个函数，使事情变得容易得多。例如，代码可能是这样的。

```
def makePizza 
  puts "Rolling out pizza dough"
  puts "Covering pizza dough with sauce"
  puts "Grating the cheese"
  puts "Covering pizza dough with cheese"
  puts "Preheating oven"
  puts "Putting pizza in the oven"
  puts "Pizza is ready!!!"
end
```

现在我们不用担心披萨的制作过程是怎样的了。我们所要做的就是运行这个函数，工作就完成了。这是一种让代码可重用的惊人方法。因为你不需要理解这个过程，这也使得其他开发者更容易更快地阅读你的代码。如果我们必须理解代码中的每一个过程，那就没有工作可做了。现在下一个支柱向我们展示了实现抽象的方法。

# 包装

封装是指隐藏数据。这可以通过移除对部分代码的访问并使其私有来实现。如果不需要某些东西，你可以让它们变得更难接近。封装用于隐藏类中结构化数据对象的状态，防止未授权方直接访问它们。那么，为什么我们更喜欢这种隐私，而不是让一切全球化。嗯，有几个原因。

*   程序中完全不同的部分不能意外地更改对象中的数据。
*   该功能在一个地方且仅在一个地方定义。
*   你不会以意大利面条代码结束。

这些只是封装在面向对象编程中非常重要的几个原因。我们把数据藏在一个其他人不需要访问的地方，只显示需要的东西。这就是封装的意义所在。

# 遗产

继承让一个对象获得另一个对象的属性。如果我们知道某样东西在多个地方都需要，可能有一些小的不同，我们可以使用继承来提高可重用性。继承通常用于类之间的父子关系。我们试图让我们创建的代码尽可能地相互关联。例如，如果我们想创造动物，我们知道大多数动物在世界上行走，所以我们可以有一些类似这样的代码。

```
class Animal def initialize
    puts "This is an Animal"
  end def movement
    puts "The animal is walking"
  endendclass Dog < Animal def initialize
    puts "This is a Dog"
  endenddog = Dog.new
dog.movement // Will puts "The animal is walking" 
```

在这个例子中，我们的 dog 类从它的父类继承了 movement 方法。因此，当我们调用 dog 对象上的 movement 方法时，我们将获得与 animal 类上的 movement 方法相同的行为。使用继承的主要好处是您的代码可以重用。我们可以避免反复编写相同的代码。

# 多态性

多态意味着同一继承链中的对象可以以不同的形式出现，做不同的事情。我们可以用同一个动物类的例子来展示这意味着什么。让我们说，现在我们想创造另一类动物，如蛇。蛇可能与其他动物有许多相似之处，但它们的运动方式绝对不同于其他动物。

```
class Animal def initialize
    puts "This is an Animal"
  end def movement
    puts "The animal is walking"
  endendclass Snake < Animal def initialize
    puts "This is a Snake"
  end def movement
    puts "The snake is slithering
  endendsnake = Snake.new
snake.movement // Will puts "The snake is slithering"
```

正如你所看到的，我们能够覆盖移动方法，使它更具体地描述蛇实际会做什么。这让我们可以创建从父类继承一些东西的对象，但也可以修改一些东西使之成为自己的东西。

这是对面向对象编程的四个支柱的一个非常简单的解释。遵循这些规则已经帮助许多程序员写出了干净的面向对象的代码。真正理解这些原则，会让你成为更好的开发者。享受深入研究和在代码中使用这些原则的乐趣。编码快乐！😎