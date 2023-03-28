# 数据结构与对象— Swift

> 原文：<https://medium.com/geekculture/data-structures-vs-objects-swift-a57fabbed2df?source=collection_archive---------15----------------------->

![](img/75362039b4bea1a4b730979b32e9b5b4.png)

我正在读罗伯特·c·马丁的《干净的代码》的第 6 章，我对对象和数据结构之间的区别感到非常困惑。我不得不多次阅读，以理解是什么使对象成为对象，是什么使数据结构成为数据结构，以及它们之间的区别。我花了不止一、二、三次阅读来更好地理解它，但可以肯定地说，我理解了它的主旨。然后我决定把它应用到我最喜欢的语言(Swift)上，分享我的理解和几个例子。

**数据结构:**

数据结构应该公开它们的数据，不需要有任何有意义的行为。它们应该作为容器，一种访问数据的方式，以便外部各方可以使用它们来创建有意义的行为。

Swift 中的一个例子是 Enum。枚举非常适合定义数据

```
**enum** Word {**case** circle**case** square**case** rectangle}
```

在这个数据结构中，请注意只有数据是公开的，没有依赖于使用它的真正的行为或功能。这样做的好处是，使用这种数据结构，你可以有许多不同的功能。

**对象:**

对象通过使用抽象来隐藏它们的数据，但是会暴露行为和函数。这可以通过使它们的字段私有，同时公开将操作它们的私有数据的函数来实现。
此外，对象不同于数据结构，因为它的方法或行为不应该是访问数据的方式，而是执行操作这些数据的动作。如果一个对象有一个函数只返回它的一个私有数据的值，那么它就不是一个对象，而是一个数据结构
我这么说是什么意思呢？看一下这段代码:

```
**class** Square {**private** **var** sideLength: Int**init**(sideLength: Int) {**self**.sideLength = sideLength}**func** getSideLength() -> Int {**return** sideLength}}
```

这仍然被认为是一个数据结构，因为这个类通过“getsidelongth()”公开它的数据。为了使它成为一个对象，我们需要创建一个新的行为，这个行为不会暴露它的数据，而是操纵它，从而保存抽象的概念。

就像这样:

```
**class** Square {**private** **var** sideLength: Int**init**(sideLength: Int) {**self**.sideLength = sideLength}**func** findArea() -> Int {**return** sideLength * sideLength}}
```

注意 findArea()是如何操作数据并隐藏实现的。

**使用对象 VS 数据结构的优势和不便？**

这本书通过强调对数据结构使用过程代码和使用面向对象代码的优缺点来说明这些区别。

**使用数据结构的程序代码**

```
**enum** Word {**case** circle**case** square**case** rectangle}**class** Translation {**func** frenchTranslation(word: Word) -> String {**switch** word {**case** .circle:**return** "rond"**case** .square:**return** "carré"**case** .rectangle:**return** "rectangle"}}}
```

请注意,“枚举单词”是一个数据结构，frenchTranslation()函数对三个给定的单词进行操作。枚举单词没有行为，但是翻译类使用该枚举来创建行为。这种使用数据结构的过程化流程的优点是，我可以继续向翻译类添加更多的函数，而不会影响枚举字。然而，一旦我向枚举添加了一个新词，比如说 *case triangle* ，我添加到翻译类的所有函数都需要更新，以考虑这个新词。
因此，添加新的行为很容易，但是添加新的数据结构可能很繁琐，因为每个函数都需要适应新添加的数据结构。
现在让我们来看看面向对象的方式。

**面向对象方式**

```
**protocol** Shape {**func** findArea() -> Int}**class** Square: Shape {**private** **var** side: Intinit(side: Int) {**self**.side = side}**func** findArea() -> Int {
```