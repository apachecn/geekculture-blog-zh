# Java 中的方差介绍

> 原文：<https://medium.com/geekculture/introduction-to-variance-in-java-2c0291a1388e?source=collection_archive---------0----------------------->

## 扩展和更新版本

*这是受*[*《Java 和 Scala 差异完全指南》*](/javarevisited/variance-in-java-and-scala-63af925d21dc) 启发的扩展和更新版本

我希望我在 2008 年读到这篇文章(在我搬到 JDK 5.0 之前)。文章中提供的所有代码都在 JDK 8 上进行了测试。

如你所知，在 Java 中整数扩展了数字。字符串不扩展数字，字符串扩展对象。以下代码是合法的:

第一行定义整数 1。

第二行是标准对象赋值，*通过引用复制*，在第二行之后我们有两个引用*一个*和*另一个*指向同一个对象整数 1。

第三行也通过引用使*复制。*它利用了所谓的*多态性*。

第五行不编译。我们试图将 String 类型的引用赋值给 Integer 类型的引用。但是 Integer 和 String 没有任何继承关系(Integer 不直接或间接扩展 String，not String 不直接或间接扩展 Integer)。因此，这种赋值在编译时会失败。这就是该行被注释掉的原因。

第七行定义了包含值“one"⁶.”字符串

八行使*通过引用*在字符串之间复制。 *Str* 和 *anotherStr* 引用相同的值。

九行不编译。我们试图将字符串类型的引用赋值给数字类型的引用。但是数字和字符串没有任何继承关系。具体来说，String 不扩展任何最终(也就是上面的*间接*的意思)扩展 Number 的类。因此，这种赋值在编译时会失败。这就是该行被注释掉的原因。

Java 中的数组是*协变的。这是什么意思？*

Integer *是一个*数，Integer 是 Number 的子类。让我们将这个事实表示为整数< : Number。

在 Java Integer[] *中是一个*数字[]，或者在我们的新符号 Integer[] <:数字[]。这叫做**协方差**。这使得 6 号线能够工作。整数[]实际上是可将分配给数字[]的*。*

*注意:*在 Java Integer 中，Number，String(任何其他对象)都是 java.lang.Object 的子类，因为，Java 数组中的**是协变的**，这就意味着 Integer[] < : Object[]，Number[] < : Object[]，String[] < : Object。

第 6 行没有编译，因为赋值:

```
Integer a0="1";
Integer a1="1";
Integer a2="1";
Integer a3="1";
```

字符串不是整数的子类。

第 10 行我们定义了另一个引用来引用同一个数组(在 Java 中 array 是一个对象)。

第 11 行没有编译，原因和第 8 行一样。每个单元分配失败。

科特林和 Scala 选择他们的数组是 ***不变的。*** 字符串和数字都是不变类的例子，*它们都不是对方的子类。*

在 Kotlin 和 Scala 中，语法上等价的第 4 行不会被编译。不变类型*不可赋值*。为了理解为什么让我们看看下面的代码:

我不会深入这个代码的细节，它非常简单。有些设计的选择只是为了演示的目的(SportCar.getNumberOfWheels()是多余的，这是他们为了清晰起见；getNumberOfWheels()可以移动到 Vehicle 类中，如果没有轮子，则返回 0(例如在 rocket 或 dirigible 中，等等)。

让我们首先简单地看一下这个问题:

首先，我们实例化一些汽车数组。注意数组中的 SportCar <: car="" vehicle="" object.="" motorcycle=""/>

Car is subclass of Object. Because array in Java are covariant, array of car is subclass of array of object, and thus we can assign array of cars to array of objects. Now, compiler see arr as array of object. In Java arrays are mutable (we’ll get back to this point later), so it is perfectly fine to reassign it’s first element. As far as compiler concert the type of arr[0] is java.lang.Object, so he is happy to assign Motorcycle their⁷.

If this example fail to convenience you, let’s look on some real (sort of) code:

The interesting part is SomeService.calcNumberOfWheels() method. There is edge case, if array is null (that is handle by ternary operator, 0 is returning immediately⁸). Another edge case, if array is empty, than we have optimization⁹, we return 0 immediately.

At line 12 we are iterating over array¹⁰.

At line 13 there are actually 2 things going on: we’re getting **out** 元素(记住，Java 中的 array 是对象，所以，实际上，Car 是从 Car[]返回的)，然后我们**将数组中这个元素的引用**分配给 cur reference。例如，我们从数组中取出 SportCar 引用，由于多态性(如上所述)，对 Car 引用的赋值将会成功。

在第 14 行，我们将 getNumberOfWheels()的结果存储在临时变量 cur 中。

在第 15 行，我们利用了允许 add 将 int 加到 long 上的`[JLS8 §5.6.2](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.6.2)` (在加法之前有 int cur 到 long 的隐式转换)。

在方法结束之前，我们要进行检查，确保没有溢出。

这里有很多细节，让我快速总结一下。我们有以下签名的汽车服务:

```
public int calcNumberOfWheels(Car[] cars)
```

这个方法应该做什么？它假设遍历汽车数组(我们一次从中取出**)，调用它的一些方法并总结返回的结果。car 的数组可以包含 Car 的子类，因为多态，所以不会有问题。**

我们能不能把**放在**里不是从 Car 衍生出来的东西？简短的回答是:不。更长的回答如下:

我们可以通过写下这样的内容来欺骗编译器:

这段代码将会编译。然而，由于 Java 数组内置的特制保护机制*，它将在运行时失败。缺点是，每次访问数组时，我们都要做一些额外的验证(这会降低程序的执行速度，执行程序的时间会不断增加)。*

在这一点上，你应该注意到，我不时地强调“在 Java 中”一些陈述是正确的。但我说的是 Java，何苦呢？如今，JVM 不仅可以运行 Java，还可以运行 Groovy、Scala、Kotlin 等。我不会详细说明这是如何实现的，我只会说，所有这些语言都编译成相同的*字节码，*但有不同的编译器。所有这些语言都是在 Java 之后很久才出现的，所以看看他们做了什么选择是很有趣的。

Scala 和 Kotlin 认为数组是不变的。原因如上所述，否则，你的代码可以编译，但在运行时会崩溃。对于 Java 来说，改变这个设计决策为时已晚。当*泛型*在 JDK 5.0 中被添加时，Java 团队认识到了这种设计方法(稍后将详细介绍)。它们被声明为**不变量。**例如，列表<跑车>不可分配*给列表<汽车>。我们稍后将回到这一点。*

Java 团队还有其他选择吗？我知道，由于*向后兼容性*的考虑，我们不能真正实现任何替代方案(这会破坏现有的代码)，但是让我们把这当作理论练习。

让我们仔细看看 calcNumberOfWheels()的代码。回想一下，Java 中的数组是**协变的。**但是当我们从数组中取出**和**元素时没有任何问题(因为多态性)。我们确实有问题，只是我们将**放在**的一些实例中(如上所述，Java 有特殊的内置保护机制)。因此，另一种选择是让数组能够从元素中获取**(通过索引，或者通过一些其他的能力),而不能让**改变**元素。这样的对象叫做**不可变**。**

**不可变**对象是在构造完成后不改变其*状态*的对象。也就是说，在构造函数执行结束后，对象的*状态*不应该改变。如果我们禁止重新分配数组的元素(代码如

```
arr[0]=some_instance
```

将是非法的)，考虑到 Java 中的数组没有 add(数组是 Java 可以动态增长的)和 clear(这两种方法也会改变状态)这样的方法，这样的数组将是**不可变的。**看起来**对于不可变对象是协变的是安全的。稍后我会详细说明。**

到目前为止，我们看到数组可以是**协变的**。我们还简要地提到，默认情况下，泛型(又名参数化类型)是**不变的**。现在，我想指出我们也有**协变返回类型**(方法的)。
让我们再来看看我们的汽车等级体系:

现在让我们把重点放在 withDriver()方法上。让我们看看这个方法的实际用法是怎样的:

首先，我们定义新汽车实例。然后，我们希望将驱动程序分配给它，并在 car 实例上调用一些 Car 特定的方法。

我们真正想要的是编写第 3 行，只是为了调用 withDriver()方法。也许我们想写这样的东西:

```
car.withDriver(driver)
   .someCarMethod()
```

*边注:*这么做的原因之一是写[*fluent API*](https://en.wikipedia.org/wiki/Fluent_interface)*(最简单的例子是 StringBuilder，比较典型的例子是 Stream API 或者 CompletableFuture)。*

*可惜这个不编译。原因是 withDriver()方法的返回类型是 Vehicle(不是 Car！).虽然我们知道*实际上(*我们返回它)在运行时会被 Car，但是编译器不知道这一点。克服这个障碍的一个方法是添加显式强制转换，就像我们在第 4 行所做的那样。它看起来不漂亮，但这个工作。*

*我们能有更好的解决方案吗？从 JDK 5.0 开始就有了。*

```
*car.withDriver(driver)
   .someCarMethod()*
```

*也会起作用。*

*为了理解这一点，我们需要了解 Java 中的**方法重载** ⁴是如何完成的，以及什么是**桥接方法** ⁵.*

*   *具有相同签名(方法名+方法参数)但不同返回类型的两个方法是编译时错误。*
*   *然而，在运行时，JVM 允许两个方法具有相同的签名，但返回类型不同。*

*让我们看看 Car.withDriver()方法。让我们问自己一个问题，这个方法是覆盖还是重载 Vehicle.withDriver()方法？显然，我们想要覆盖，这是注释@Override 用于⁴.但是，这是如何实现的呢？*

*在 5.0 之前，当您覆盖超类方法时，被覆盖方法的名称、参数类型和返回类型必须与超类方法完全相同。据说覆盖方法对于参数类型和返回类型是**不变的**。*

*如果您更改了任何参数类型，那么您并没有真正地覆盖一个方法—您实际上是在重载它。*

*例如，如果我们添加 someCarMethod(Car another):*

*这将导致**方法过载**。*

*让我们再来看看方法签名:*

*在 JDK 5.0 之前，这里会有编译时错误。在 JDK 5.0 中有所改变。现在，*你可以在子类中有不同的返回类型，只要这个返回类型是超类返回类型的子类。在我们的例子中，Car < : Vehicle 既是 withDriver()方法的返回类型，也是继承树中的子类。**

*方法覆盖被认为是相对于返回类型的**协变。***

**注:*再比如克隆法。Object.clone()方法返回对象类型。*

**注意:*有趣的是，**异常声明甚至在 5.0 之前就已经协变**。也就是说，子类方法可能抛出超类方法异常的相同类型或子类型。*

*现在，我们有了理解**协变返回类型**是如何实现的所有要素。*

*实际上，java 编译器正在做的是将合成/桥接方法添加到子类中(在我们的例子中是 Car):*

*你不能写这样的代码，编译器会产生编译时错误。但是编译器被允许这样做。*

*所以，实际上，编译器的桥方法覆盖了车辆的原始方法。这个桥方法调用实际的实现。实际的实现方法重载了桥方法。从实际实现返回的值(在我们的例子中是 Car)是 bridge 方法(Vehicle)的子类，所以通过多态我们可以从 bridge 方法返回子类(为了清楚起见，在第 4 行通过临时变量完成)。*

*现在，让我们考虑**通用**和**集合**。*

*先说说 List <car>和 Car[]的区别。实际上，有许多点可以区分它们，我将只列出其中几个(基本类型、空值等被跳过):</car>*

*   *Car[]无法动态增长。您不能添加超过其长度新车(您只能显式地复制到新创建的更大的数组)。
    -如果您使用 LinkedList 实现，您可以自由添加/删除新项目(达到内存限制)。
    -如果使用 java.util.ArrayList 实现，这种动态增长将隐式完成。
    -java.util.Arrays.asList()返回固定大小的列表，他不能动态增长。(你可以把它传递给 java.util.ArrayList 类的构造函数来克服这个缺点)。
    -JDK 9 的 java.util.List.Of()是 java.util.Arrays.asList()的优化版本。*
*   *-At Car[]-数组中项的实际类型是 Car。运行时(JVM)知道这一点。存在上述存储错误类型的嵌入式防御机制()。
    -列表<车>-列表中项目的实际类型为对象(由于*类型擦除* ⁷).编译器尽最大努力确保列表包含正确的类型，但是它可以被欺骗(我将在后面展示几个这样的技巧)。*
*   *-列表<car>是**不变的**。
    -Car[]是**协变**(如上所述)</car>*

*我想进一步阐述最后一点。*

*List <car>是不变的——这意味着，虽然 SportCar <: car="" we="" can="" determine="" relationship="" whether="" list="">或 List<: list="">或<car>或<:>。为什么 Java 有这样的易变性？</car></car>*

*简而言之:是*设计选择*。*

*更长的答案如下:一般来说，在面向对象编程语言中有两种支持泛型(也称为参数化类型)的技术。第一种技术，**同构转换**，将泛型类型解析为单一类型，而不考虑其类型参数。这是 Java 使用的技术，由此`List<Car>`和`List<SportCar>`都解析为运行时类型`List` 【类型被*擦除*】。第二种技术，**异构翻译**(或**泛型特殊化**)，导致单个泛型类型的不同类型参数在运行时解析为不同的类型(更多细节请参见 [C++模板类](http://en.cppreference.com/w/cpp/language/templates))。例如，`List<String>`和`List<Integer>`将分别产生类似于`List_String`和`List_Integer`的运行时类型(与单一的同类类型`List`相反)。*

**边注:*截至 2019 年[瓦尔哈拉项目](https://openjdk.java.net/projects/valhalla/)仍在进行中。*

**因为 java 选择用类型擦除实现泛型，所以我们不能像对数组*那样实现运行时保护，因为我们在运行时只有一个包含 java.lang.Object 的类列表。这就是为什么 Java 中的**泛型在默认情况下是不变的。***

*同样，在 Scala 和 Kotlin 中，数组在默认情况下是不变的，list 和 collection 也有所不同。*

***Scala 中的链表是不可变的⁸和协变的。***

***在 Kotlin 列表中是只读的⁸和协变的。***

**注意:*您可能会注意到集合的不可变能力和只读能力之间存在一些差异，但是这种差异对于*差异*的讨论来说并不重要，所以我不会深入讨论这个问题。*

**注意:* Kotlin 和 Scala 总体上同意**同构转换**技术是正确的选择，但是他们发现在一些重要的特殊情况下(集合是不可变的/只读的)我们可以有**协变集合。我们将在后面的故事中探讨为什么会这样。***

*让我们回到 Java，看看一些使用泛型的实际代码示例。这将是列表，但这只是一个方便的例子。我们还将重用上面的代码:*

*现在，让我们向 SomeService 添加新方法:*

*这是主要的:*

*主要方法是 SomeService.hasSameDriver()。让我们一行一行地过一遍。*

*首先，我们检查列表的大小。如果列表为空，那么我们假设，该列表具有相同的 driver⁹.如果列表为空或者比我们假设的多 1 个项目(长度≤1)，则该列表具有相同的驱动因素。否则，我们知道列表有不止一个条目(长度> 1)。*

*第 15 行非常有趣。我们从列表中获取 **out** iterator。调用 iter.next()(之前没有检查 iter.hasNext())是安全的，因为我们知道长度> 1。*

*第 16 行**产生列表中的第一个项目**。⁰或者，我们写道*

```
*Car first = cars.get(0);*
```

*这是可行的。我在这里使用迭代器有两个主要原因:*

*   *hasSameDriver()方法可以对任何集合起作用。例如，我们可以将签名更改为 hasSameDriver(Set < Car >)，代码无需任何更改即可工作(Set 没有 get(0)方法)。*
*   *我们将重用迭代器从第二项开始遍历列表。*

*在第 17 行，我们从第一辆车中检索出司机。我们会把它和其他车里的司机的做比较。*

*注意:我们可以将其与列表中每辆车的每个驾驶员进行比较。第一次比较将只是“一种浪费”。⁰*

*第 21 行确保我们遍历迭代器，直到底层列表用尽。替代方法是:*

```
*Car car = null;
for(Iterator<Car> iter = cars.iterator;iter.hasNext();){
   car = iter.next();
   ...}*
```

*注意:在 JDK 5.0 之前，上面演示的方法是我最喜欢的。在我们的例子中，我们需要在 for 循环之前初始化 iter。因此，只剩下检查退出条件。虽然从技术上来说，这是可能的*

```
*for(;iter.hasNext();)*
```

*我把它改成了 while 循环。*

*第 22 行我们正在从当前的汽车中检索司机。现在，我们调用帮助器方法 isEquals()，它将检查我们是否有相同的驱动程序。如果我们有不同的驱动程序，我们将终止循环迭代并返回 false，我们在循环中发现了不同的(与第一个不同的)驱动程序。*

*当我们完成迭代并且没有发现任何与第一个不同的驱动因素时，我们可以实现第 119 行。因为等于的传递性意味着所有的驱动都是一样的，所以我们要返回 true。*

*我不会深入讨论 helper 方法 isEquals()实现的细节。基本上，驱动程序没有覆盖 equals()方法，所以它从 java.lang.Object 继承了一个⁹，该对象只使用身份检查(==)。*

**注意:*我把空驱动当做特殊值。如果所有的车都有空的司机，我认为他们有相同的司机。*

**注意:* isEquals(Object，Object)可以改成 isEquals(Driver，Driver)。一切都会好的。让 equals()的重载版本接收除 java.lang.Object ⁰.之外的任何其他值是一个设计错误因此，在驱动程序的层次结构中，每个类中应该只有零个或一个 equals()(本质上，我们从 java.lang.Object 中重写了 equals())。因此，在运行时，将调用 equals()的最后一个被覆盖的版本(或者是 java.lang.Object 中的默认版本，如果我们没有覆盖 equals())。*

*让我们来看看主函数。*

*第 7 行是有趣的部分。这里我们实例化*

```
*List<Car> cars*
```

*我们使用 java.util.Arrays.asList()方法从单个项目创建参数化列表。注意，为了简单起见，我们只是使用默认构造函数来定义列表中的项目(这意味着，驱动程序将为空)。*

*注意:如果您使用的是 JDK 9 和更高版本，您可以使用 java.util.List.of()函数。这是一个重载函数，根据你想要放入列表中的条目数，返回更优化的列表实现。*

*现在，是时候告诉你，从类型用法的角度来看，上面的代码是错误的。*

*第 12 行实际上是不编译的！编译器发出以下错误信息:*

> *SomeService 类型中的方法具有 SameDriver(列表<car>)不适用于参数(列表<sportcar></sportcar></car>*

*基本上是说，为了调用 hasSameDriver()方法，我们应该进行*赋值*来列出< Car > cars 参数。我们传递的是 List<sport cars>sport cars。编译器无法进行赋值。为什么？因为 Java 中的**泛型类型是不变的**。*

*但是，如果您检查 hasSameDriver()方法的主体，您会注意到有趣的调用是 cars.size()、cars.iterator()、iter.hasNext()、iter.next()。您可以合理地预期，在运行时，代码执行应该是相同的。如果我们只能对 Java 说，我们希望我们的**列表< Car > cars 参数是协变的**。之前，我会告诉你怎么做，让我们黑类型系统。*

*这是最简单的方法:*

*java.util.Arrays.asList()具有以下签名:*

```
*public static <T> List<T> asList(T... a)*
```

*这就是所谓的*通用方法。* Java 在这里使用(某种有限版本的)类型推断来计算 T 应该在编译类型替换为 Car(因为我们在赋值端有 List < Car >，我们看到该方法返回的类型是 List<T>；由于多态性，我们可以将传入方法(T…)的每个 SportCar 实例视为 Car。因此，Java 编译器从 SportCar 项目中创建正确的 Car 列表。*

*你可以试着这样写:*

*但是在第 6 行，编译器会发出以下错误消息:*

> *类型不匹配:无法从列表<sportcar>转换到列表<car></car></sportcar>*

*此时，您应该明白，编译器告诉您，因为 List <sportcar>与 List <sportcar>不是共变的，所以强制转换总是会失败，所以编译器会拒绝代码。</sportcar></sportcar>*

*让我们试试这个方法:*

*这段代码可以工作，但是我们有两个警告。在第 6 行我们有*

> *列表是原始类型。对泛型类型列表<e>的引用应该参数化。</e>*

*第 7 行有以下警告:*

> *类型安全:类型列表的表达式需要进行未检查的转换以符合列表<car></car>*

*基本上，编译器告诉你的是:“我看到你在使用传统的原始类型。您应该真正考虑使用参数化类型。然后他说:“嗯，现在，我看到你在我身上下功夫。我不能保证 someSportCars 里面只包含汽车，但我不能阻止你(因为遗留代码(JDK 5.0 之前的考虑)”。*

*这是可行的，但是编译器在对我们大喊大叫。我们能闭嘴吗？是的，我们可以！*

*现在，代码工作了，我们没有看到任何警告。*

*我向你展示这种技术，因为偶尔你会需要使用一些上述技巧的变体。例如，java.util.ArrayList 的实现如下所示(只保留一小部分代码):*

*实际数据存储在 Object[]中。对它的所有操作都是通过 helper 方法 elementData()完成的。*

*在编译时，泛型参数类型 E 是未知的(因为上面提到的**同构转换**技术)。泛型参数类型 E 在运行时也不可用。因此，在运行时有效地将对 E 的强制转换替换为对 Object 的强制转换(这没有任何作用(列表不能包含原始类型，至少在 [Valhalla 项目](https://openjdk.java.net/projects/valhalla/)完成之前是这样)，所以所有东西都是对象)。编译器再次足够聪明，所以它发出警告。我们取消了警告，有效地绕过了类型安全检查。*

*那么，解决办法是什么呢？我们如何向 Java 编译器表达我们希望我们的列表<car>是协变的？我们应该使用</car>*

```
*List<? extends Car> cars*
```

*这是汽车类型的协变列表。这是要付出代价的。现在编译器不允许把**放在**里。这是一种只读列表。*

*我要重新迭代，List extends Car>是**协变**(排序)**只读** list。*

*   *列表**与汽车类型**共变，这意味着如果我们有列表< SportCar >编译器将允许我们将其赋给汽车。*
*   *List 是(某种)只读的**这意味着编译器会尽最大努力阻止我们往里面放新东西。***

*和主类:*

*在第 9 行，我们已经修改了我们讨论过的方法签名。*

****注:*** *据说方法 hasSameDriver()有* ***协变*** *参数。**

*这个变化导致了第 15 行的一些变化，我会稍作解释。*

*在主类中，我们现在同时传递 List <car>(第 7 行)和 List <sportcar>(第 11 行)，因为 cars 参数是**而不是协变的**。</sportcar></car>*

*现在，我们要付出的代价是这个列表是只读的。什么意思？在定义 cars 变量的范围内(在 hasSameDriver()方法内), Java 编译器会尽力阻止我们调用任何接收参数类型(Car)作为其参数之一的方法。举个例子，*

```
*public boolean hasSameDriver(List<? extends Car> cars){
   ...
   cars.add(new Car());
   ...
}*
```

*不会编译。编译器将发出错误消息，指出:*

> *该方法添加(捕获#15-of？类型列表<capture extends="" car="">中的扩展汽车)不适用于参数(汽车)</capture>*

*回想一下列表的定义。这是不完整的定义(跳过了一些方法):*

*编译器基本上是这样说的:“你定义**协变**(这有什么好笑的*捕捉#15-of？用参数化类型(" E ")汽车扩展*表示(扩展汽车)。你试图调用方法 java.util.List.add()，但是这个方法需要类型 *capture#15-of？延伸汽车。*你试图向它供应汽车，但这不适用。”原因是，类型擦除，我们没有运行时防御机制(就像我们对数组一样)，所以编译器拒绝将*任何东西*(任何东西，除了 null)作为 java.util.List.add()的参数。*

*注意:*

1.  *您可以调用任何不接收“E”作为参数的方法。*

*   *比如可以调用 *cars.size()。*没有问题。*
*   *也可以调用 *cars.clear()。*它确实改变了列表的状态。编译器不知道不变性/只读。编译器关心的是，是否“E”作为参数被接收，所以编译器允许这个调用，同时改变列表。作为最佳实践，通常应避免此类呼叫。*

1.  *您可以传递 null 来代替“E”。因此调用 cars.add(null)将实际编译(并将被执行)。*
2.  *您可以使用强制转换来破解编译器(见上文)。所以，如果你真的想要*，你可以绕过类型安全编译器检查*。*
3.  *您可以调用返回“E”的方法(我们从类型为“E”的对象中获取**，这些**方法具有协变的返回类型)**。下面一行***

```
*Car first = cars.get(0);*
```

*完全有效。可以看到 java.util.List.get(int)返回 e。*

*编译器害怕的是，你会用“错误的类型”来放置对象，比如 *cars.add(new Vehicle())。*同样，在运行时我们不能检查插入对象的类型和“E”(在 java.util.List < E >中)是否一致。在编译时，编译器不能区分:*

```
*Vehicle vehicle = new Car();
cars.add(vehicle);   //should be ok, at runtime it is Car*
```

*和*

```
*Vehicle vehicle = new Vehicle();
cars.add(vehicle);   //should be rejected*
```

*在这两种情况下，编译器看到类型 Vehicle 将要被插入到 cars 中。因此，编译器选择拒绝任何尝试(除了 null)。*

*现在，让我们回到第 15 行。在第 15 行，我们调用 java.util.List.iterator()方法。它返回迭代器<e>。那么，我们有了**的协变**列表，“E”就是*捕捉的第 15-15 个吗？延伸汽车。*因此，迭代器对象也应该使用与 list 相同的“E”进行参数化，即迭代器对于 Car 类型也是**协变的**。那到底是什么*迭代器<？延伸车> iter* 的意思是。</e>*

*java.util.Iterator 有 2 个抽象方法:*布尔 hasNext()* 和 *E next()。*很明显，第一个方法是“独立于”E 的，所以我们可以自由调用它。在 next()方法中，我们从类型为的对象中获取**，这个**方法具有协变的返回类型，**，因此可以调用它，并且我们可以断言我们将接收 Car 的实例。***

*现在，该谈谈**逆变式了。***

*先从定义说起。鉴于一个<: b="" a="" is="" subclass="" of="" if="" t="" then="" class="iz hj">逆变在其型。*

*先说例子。我将重构我们的 SomeService。我会做以下的改变。我们将分两步进行重构。在第一步之后，我们将在第二步中修复代码中的一些错误。*

*首先，我将协变参数从列表 extends Car>改为列表 extends Vehicle>。如果你仔细观察，你会发现我们从 cars 中对 item 调用的唯一方法是 *getDriver()* 这实际上是定义的 Vehicle。我还将把本地变量名(和类型)从 *Car car* 改为 *Vehicle vehicle* 。*

*我将添加新的(重载的)方法，该方法将 BiPredicate 作为附加参数。旧方法将使用 isEquals()实现，客户端将能够提供不同的实现。双预测在两个参数中都是逆变的。*

*客户端代码:*

*在代码的第 43 行，有一个旧方法 *hasSameDriver()* ，它的签名略有变化。在第 44 行，我们使用 Java 8 方法引用。现在，当 comp.test()将被调用时，Lambda mechanic 将调用 *SomeService.isEquals()* 方法。这将确保 this 1-parameter 方法会像以前一样精确地比较列表中的项目。注意，当 isEquals()接收 2 个 java.lang.Object 时，BiPredicate 的赋值<？超级司机，？超级驱动程序>工作正常，因为 BiPredicate 在它的两个参数中是**逆变**，它*接受*驱动程序和所有 it 祖先(包括 java.lang.Object)。*

*实际的实现被转移到新创建的双参数 *hasSameDriver()* 方法*中。*列表<中第一个参数的签名被更改？将轿厢>延伸至列表<？扩展了 Vehicle >，现在这个**方法是*协变*在它的第一个参数与类型 Vehicle** 列表中。第二个参数是 BiPredicate，在它的两个参数中是**逆变**，它*接受*驱动程序和所有 it 祖先(包括 java.lang.Object)。你能指出签名中的瑕疵吗？下面我给你解释一下。*

*在第 17 行，我添加了先决条件检查 com 不为空。在 comp 为 null 的情况下，我们实际上可以使用 isEquals()。这是设计的选择。*

*有多个小的变化(车辆代替汽车，和变量名的变化)。主要的变化在第 33 行。它实际上被注释掉了。之前，我们简单地调用了我们的私有 isEquals()方法，现在我们使用的是 comp 对象，它的两个参数是**逆变**。*

*现在，让我们看看客户端代码。注意，第 7-20 行没有改变。我提醒你，我们已经改变了“旧”方法的签名，现在它接收车辆及其祖先的列表，但是创建车辆列表的旧代码继续工作，没有改变。Vehicle <: car="" list=""> <: list="">，所以我们*扩展了【hasSameDriver()可以在其第一个参数中接收什么类型的对象。**

*现在，让我们看看第 22-29 行。出于演示的目的，我写了 Lamda 表达式的 SAM 类型和参数的完整签名，在生产代码中，所有这些都可以省略。我这样做是为了演示我们可以将双预测和双预测<driver driver="">作为第二个参数传递给 *hasSamedDriver()* 方法。BiPredicate 的实际实现是引用比较的捷径，只有当您只有几个驱动程序，并且没有创建新的驱动程序(例如，如果驱动程序是 Singleton)并使用某种查找机制来找到合适的驱动程序(在 Singleton 的情况下是 getInstance()静态函数)时，它在实际代码中才有用。</driver>*

*第 32–38 行中的代码只是演示了当我们有不同的实例时代码可以工作。现在能识别出 *hasSameDriver* ()方法签名中的问题吗？请看第 33 行的提示。*

*实际上，有两个不同点需要考虑:*

*   *我们允许像双预测<driver object="">这样的签名吗？也就是说当第一个和第二个参数不同时(但是一个是另一个的实例)？</driver>*
*   *司机的祖先呢？我们也应该允许他们吗？换句话说，*是否有一个名称驱动*()在它的第二个参数中限制太多。*

*例如，我们可能在某处定义了以下类:*

*第一点有些微妙。*

*首先，让我们注意，像 BiPredicate <driver car="">这样的东西，当我们有两个不相关的(在它们的继承树中)参数时是没有用的。我们不能比较不相关的例子。</driver>*

*因此，**它的一个参数是另一个参数的实例。***

*现在，如果我们在 Driver 上也有一些继承层次，我们的 *hasSameDriver()* 应该肯定能够使用它(提供适当的 *comp* 对象)。*

*为了演示第二点，让我们考虑下面的代码:*

**Automate* 与*不兼容？超级司机*。*

*总的来说，我们希望在两个参数中双预测为类型驱动程序的**逆变，我们希望一个参数是另一个的实例，并且我们希望允许驱动程序的任何祖先被接受。也许我们在这里有些矛盾？让我们看看。这让我们想到了下面的签名:***

*？超级 T-表示逆变参数(两者)*

*t 扩展了驱动程序的含义——我们也允许接收驱动程序的祖先。*

*注意:最好有不同的类型，例如双指示符<driver object="">。参见下面的例子。</driver>*

*现在，我们遇到了一些困难。如果你只改变了 *hasSameDriver()* 的签名，代码将不会被编译。原因与编译器应该添加的重载方法和桥方法有关。编译器不知道是否要重写*

```
*boolean test(Object t, Object u)*
```

*或者*

```
*boolean test(Driver t, Driver u)*
```

*或者*

```
*boolean test(Automate, Automate u)*
```

*它可以是这些中的任何一个。假设您有实现双向接口常规类。现在，这个类已经重载了*测试*()方法。这些方法中只有一个可以覆盖实际双预测方法。但它可以是这些中的任何一个。现在，当 *hasSameDriver()被*编译时，其中一个必须被静态链接。但是编译器不知道选择什么和如何选择。因此，在第 23 行，当我们调用对 *test* ()方法的调用时，编译器将发出**错误。***

*注意，在实践中，BiPredicate 在大多数情况下将被实现为 Lambda 表达式。如果没有，实际上它仍然只有一个 test()方法。所以，我们要对编译器说:“嘿，随便调用*任何* test()方法就行了”。因为，我们知道，只有 1，一切都会按预期运行。*

*但是如果有人真的编写了重载的*测试*()方法。那么，在这种情况下，它们应该*兼容*在某种意义上，一个应该调用另一个(或做同样的事情)来获得适当的运行时类型，例如，参见 Java . lang . timestamp . equals(Java . lang . object)和 Java . lang . timestamp . equals(Java . lang . timestamp)⁰.如果实现的类是以这种方式设计的，它将“修正”编译器可能的错误选择，所以在这种情况下，我们可以在编译时只选择*任何* test()方法。*

*如果有人编写了重载的*测试*()方法，而这些方法在上述意义上是*不兼容的*，那么编译器是对的，我们什么也做不了。*

*所以，我们会假设双预言是“好的”，最好是 Lamda 表达式，而如果不是，我们会责怪给我们“不好的”实现的人。*

*我们应该把我们的决定通知编译器。代码如下:*

*请注意第 24–25 行和第 36 行。我们正在对双预测进行未检查的强制转换(它是未检查的，因为编译器无法在编译时验证它，所以它会发出警告；我们知道这一点，所以我们取消了第 24 行的警告)。我们使用这个编译时类型来调用 test()方法。从技术上讲，编译器将寻找*

```
*boolean test(Object t, Object u)*
```

*如果只有一个名为 test()的方法，带有两个参数，但是具有不同的签名，那么它将被解析为这个方法，这是没有问题的。如果有多个*不兼容*的，那么选择可能是错误的(我们刚刚抑制了来自编译器的警告)。*

*同样，在实践中，这应该是可行的。*

*现在，让我们看一下客户端代码:*

*在第 46 行之前，这是旧代码，应该可以继续工作。*

*在第 46–48 行，我们将驱动程序改为(唯一的)Automate。我们这样做是为了确保我们能够通过双认证<automate automate="">并且这将会起作用。这在第 48–58 行完成。</automate>*

*第 60–62 行演示了这种情况，我们用两个相关但不同的类型进行了双预测。该实现看起来像是人工的，因此，我将提供 java.util.Calendar 类的真实实现:*

*所以，*Java . util . GregorianCalendar*实现*Comparable<Calendar>*即 *compareTo()* 在 Calendar 和 Gregorian Calendar 之间进行比较。它大致相当于:*

```
*BiPredicate<GregorianCalendar, Calendar> comp = (gregorianCalendar, calendar) -> gregorianCalendar.compareTo(calendar)==0;*
```

*同样，GregorianCalendar 有 compareTo()方法，该方法使用**contra varain**type Java . util . calendar 进行比较(我们将在后面看到更多)。因此，在双预测中接收不完全相同的类型应该是合法的。*

*第 64–66 行被注释掉，因为 Car 与 Driver 没有继承关系，并且我们有一个约束，即 BiPredicate(两个)参数类型必须是 Driver 的实例。所以，这不是预期的编译。*

*正如我们看到的，声明某个类型为**协变**会对它施加一些限制。**逆变**类型有什么限制？*

*注意:*

1.  *您可以调用任何不接收“E”作为参数的方法。*
2.  *您可以传递 null 而不是“E”作为参数。*
3.  *调用返回“E”的方法是*不好的做法*(我们应该只把**放在**的东西里)。下面一行*

```
*Car first = cars.get(0);*
```

*不会编译。如果你真的想把东西拿出来，你可以写作*

```
*Object first = cars.get(0);*
```

*这将编译并运行。您在这里丢失了类型信息。*

*你可以写:*

```
*Car first = (Car) cars.get(0);*
```

*但是您可能会在运行时收到 **ClassCastException** (返回对象的类型是可用的)。*

***逆变**式应该只有*“消耗”“E”。*例如，Comparable < E >类型有接收参数类型的方法，但没有返回 E 的方法。*

*现在，让我们看看**协变**和**逆变**类型的另一个例子。*可比< E >是*的例子**逆变**型(至少，是其本意)**。让我们来看看它的定义:***

*根据该定义，不清楚可比<e>的预期用途是作为**逆变**类型。正如你从 *GregorianCalendar* 例子中看到的，它是**逆变。**让我们引用 JEP 300:</e>*

> *JEP 300:用声明站点默认值增加使用站点差异*
> 
> *增强 Java 编程语言，以便泛型类或接口声明可以指示每个类型参数在默认情况下是不变的、协变的还是逆变的，从而允许类或接口的参数化之间更直观的子类型关系。这补充了现有的差异机制，通配符，这是在类型使用站点编写的。*

*[https://openjdk.java.net/jeps/300](https://openjdk.java.net/jeps/300)*

*首先，这个 JEP 仍然是唯一的候选人。截至 2019 年，该计划尚未实施。*使用地点差异*是我们目前看到的。客户代码，使用协变/逆变类型的代码声明我们希望它是协变/逆变的。*

*   *双预测<t u="">本身是**不变量**。当我们声明它是双预测的时候？超 T，？超 T >就变成了**逆变**。</t>*
*   *列表<e>是**不变量**。列表<？延伸车辆>使其**协变**。</e>*
*   *列表<e>是**不变的**。列表<？超级 T >使其**逆变。**</e>*

*Java 中没有办法在 Comparable <e>类的定义类型中表达我们希望它被用作**逆变**类型。其他一些语言(Kotlin、Scala)确实有这样的特性。这就是所谓的*申报现场变动*。</e>*

*因此，Comparable <e>是 Java 中**逆变**类型的例子(但只在使用现场)。双预测<？超 T，？超 T >也是**逆变。**</e>*

*列表 extends Vehicle>是**的协变**类型。*

*Java 有**协变**方法返回类型。*

*列表<e>在 Java 中是**不变量**。它实际上是在 Generic 添加之前添加到 JDK 的。正如我们前面讨论的，Kotlin 和 Scala 有只读/不可变列表。他们不应该在被创造之后改变。你只能从中获得**出**的物品。在 Java 中我们有*Java . util . collections . unmodifieablelist(List<？扩展 T >列表)。*它返回正则不变列表< T >，但是它覆盖了每一个打算做列表修改的方法(比如 *E set(int index，E element))* ，并抛出 UnsupportedOperationException。所以，你有*运行时保护，只有修改*。</e>*

**列表<？超级*车辆 *>是* ***逆变*** *型。**

*给定一个<: b="" a="" is="" subclass="" of="" if="" t="" then="" class="iz hj">在其类型中协变。*

*如果你想设计**协变**类型，你应该只有将**放入**这种类型的方法(比如 *add(E))，*，而不要有 *E get(int)这样的方法。**

*给定一个<: b="" a="" is="" subclass="" of="" if="" t="" then="" class="iz hj">逆变在其类型中。*

*如果你想设计 ***逆变*** 类型，你应该只有从项中取出**的方法(比如 *E get(int))，*，不要有 *add(E)这样的方法。****

*让我们看看 java.util.Collections 中的(简化)函数。*

*从 max()函数的签名可以看出，coll 在 T 中是**协变的**，在同一个 T 中是**逆变的**。请注意，从技术上讲，您可以将签名更改为*

```
*public static <T> T max(Collection<? extends T> coll, Comparator<T> comp)*
```

*或者去*

```
*public static <T> T max(Collection<T> coll, Comparator<? super T> comp)*
```

*证据是:*

*还有这个重载版本:*

```
*public static <T extends Object & Comparable<? super T>> T max(Collection<? extends T> coll)* 
```

*这个版本是等效的(从 JDK 8 开始，JDK 5.0 实际上需要显式约束 t 来对象⁴):*

```
*public static <T extends Comparable<? super T>> T max(Collection<? 
extends T> coll)*
```

*这也相当于:*

```
*public static <T extends Comparable<? super T>> T max(Collection<T> coll)*
```

*还有一个更有趣的函数 *copy()* 。下面是它的简化实现:*

*这里 src 是**协变**(实际上，我们是从项中获取**，dest 是**逆变(**它的作用类似于 sink，它也*消耗*项*)。****

*这个版本相当于:*

```
*public static <T> void copy1(List<? super T> dest, List<T> src)*
```

*这也相当于:*

```
*public static <T> void copy2(List<T> dest, List<? extends T> src)* 
```

*我发现了关于方差和逆变的很好的总结:*

> *珍指出，在《暮光之城》系列小说中，所谓的“狼人”(他们不会在满月时变形，因此实际上不是狼人)以狼和人的形式维持着他们严格的社会秩序；在社会秩序关系中，人到狼的投射是协变的。她还指出，在高中，编程语言极客处于社会秩序的最底层，但对成年的预测将他们推上了社会秩序的顶端，因此，成长是矛盾的。我对后一种说法有些怀疑；前者，我相信你的话。我想社会秩序如何在青少年狼人中运作的问题是一个额外研究的主题。*

*[https://blogs . msdn . Microsoft . com/ericlippert/2009/11/30/what-the-difference-between-co variance-and-assignment-compatibility/](https://blogs.msdn.microsoft.com/ericlippert/2009/11/30/whats-the-difference-between-covariance-and-assignment-compatibility/)*

**脚注:**

*您可以将第一行写成*

```
*Integer one=1;*
```

*并且将使用自动装箱功能(在 Java 5.0 中添加)，它实际上相当于 Integer.valueOf(1)。截至 2019 年[瓦尔哈拉项目](https://openjdk.java.net/projects/valhalla/)仍在进行中。*

*再次， [JEP 169](https://openjdk.java.net/jeps/169) 仍在进行中。*

*在我们走进*polymorphism⁵之前，我们先来简单说说*的传承*。
在面向对象编程中，继承是将一个类建立在另一个类的基础上，保留类似实现的机制。也定义为从现有的类(超类或基类)派生出新的类(子类)并将它们组成类的层次结构。**

**有几种机制可以实现*继承。*以 C++为例，使用虚拟表实现。简而言之，*c++中的每个类都是 struct 的扩展——它有字段(也称为数据成员)和方法(编译成常规函数)。*数据成员集合定义了对象的*状态*。每个类都有关联的 v 表，编译器用来确定调用哪个方法。虚拟表是在编译时填充的，每次新的类被添加到继承层次结构中时，v 表的内容都会改变。不能在类外调用的方法定义了类的*行为*。**

**在 Java 中实现机制是不同的。Java 对多重继承的支持非常有限，也就是从多个基类继承。在 Java 中你可以实现多个接口(从 Java 8 开始可以有方法体，但不能包含状态)，但你只能从一个类扩展(此外每个类都是直接或间接从 java.lang.Object 扩展而来；在 C++中没有这样的基类)。为了使事情更简单，我将只关注类，忽略接口。**

**Java 中的每个对象都有关联的类。类是您正在编写的代码，创建对象的*收据*。在运行时，JVM 创建表示对象“元数据”的 java.lang.Class 对象，这是收据的运行时表示。**

**当您在 object⁴上调用方法时，JVM 会查找具有您提供的名称和编译器从您的调用中推断出的签名的方法(类型推断超出了本文的范围)。如果在与该对象关联的 java.lang.Class 定义中找到这样的方法，则调用它。如果找不到方法，就在基类(这个类的超类)上查找。这个过程会不断重复，直到我们到达 java.lang.Object(最终，我们会到达),如果找不到这个方法，就会出现运行时错误(java.lang.NoSuchMethodError)。**

**上述算法的伪代码如下所示:**

**这个简化版来自[https://github . com/spring-projects/spring-framework/blob/master/spring-core/src/main/Java/org/spring framework/util/reflection utils . Java](https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/util/ReflectionUtils.java)。您可以查看考虑到接口并具有各种性能改进的实际实现。**

**独立于实际的实现机制，大多数面向对象语言中的继承实际上意味着通过继承创建的对象(“子对象”)获得父对象的所有属性和行为(除了:基类的构造函数、析构函数、重载运算符和友元函数)。继承允许程序员创建基于现有类的类，指定新的实现同时保持相同的行为(实现接口)，重用代码以及通过公共类和接口独立扩展原始软件。通过继承的对象或类的关系产生了一个有向图。继承是 1969 年为 Simula 发明的。**

**继承的类被称为它的父类或超类的子类。**

**不应将继承与子类型混淆。在一些语言中，继承和子类型是一致的，[这通常只在静态类型的基于类的 OO 语言中成立，比如 C++、C#、Java 和 Scala]，而在其他语言中则不同；一般来说，**子类型化建立的是一种 is-a 关系**，而**继承只是重用了实现**，建立了一种句法关系，不一定是语义关系(继承并不能保证行为子类型化)。**

**继承与对象组合相反，在对象组合中，一个对象包含另一个对象(或者一个类的对象包含另一个类的对象)；**组合实现了 has-a 关系**，与子类型的 is-a 关系形成对比。**

**可以通过委托实现继承。这有时被称为委托模式。你将你的基类存储为数据成员，你在你的子类中定义所有属于你的基类 API 的方法，当这样的方法被调用时，你只需将调用转发给数据成员。下面是一个例子:**

*   **[Kotlin](https://en.wikipedia.org/wiki/Kotlin_%28programming_language%29) 在语言语法中包含了委托模式。**
*   **Java 提供了 [Project Lombok](http://projectlombok.org/) ，它允许在字段上使用单个@Delegate 注释来实现委托，而不是复制和维护委托字段中所有方法的名称和类型。**

**⁴这就是所谓的*单分派*；当你基于单个对象查找你的方法时，这个对象通常是这样引用的(在其他语言中是 self)。**

**⁵ *多态*是 [OOPs](https://beginnersbook.com/2013/04/oops-concepts/) 特性之一，它允许我们以不同的方式执行一个动作。术语“多晶型”是指“具有多种形式”Java 中的多态性简化了编程，因为它在经历子类化的严格过程时提供了一个具有多种含义的单一接口。**

**示例:**

**上例中的 obj 是多态对象，它可以有两种变形。它可以被视为马(实际上，在运行时，在 JVM 中，它是*实际上是*马)，但它也可以被视为动物(实际上，在编译时，编译器将它称为动物)。因为马**是一种**动物，马包含动物的每个公共方法的签名。因此，编译器可以像处理 Horse 一样处理它，但是在运行时，它将被切换到 Horse(字节码的考虑超出了本文的范围)。这样的对象替换不应该中断我们的程序。这就是所谓的[利斯科夫替代原理](https://en.wikipedia.org/wiki/Liskov_substitution_principle)。**

**⁶直到 Java 9 它都是由 char[]， [JEP 254: Compact Strings](http://openjdk.java.net/jeps/254) 改变字符串的内部值，但是 API 保持不变。**

**然而，⁷在运行时会抛出 ArrayStoreException。为什么？这是从 C++中吸取的教训，添加到 java 数组中的防御机制。为了理解为什么这种防御机制是必要的，让我们假设他不在，那么下面的代码将是合法的:**

**我们已经讨论到第 3 行了。现在，让我们假设数组中没有守卫检查(例如，在 C++中就是这样)。那么第 3 行将成功执行。在第 4 行我们可以看到一些奇怪的事情:汽车数组包含了一些不是汽车的东西。好了，这意味着我们已经骗过了编译器。第 5 行演示了为什么这是个坏主意。编译器认为 cars[0]包含 car 实例，所以第 5 行编译成功。我们已经设计出了编译器，现在，有摩托车(在我们的代码中不是汽车)的代码。当 JVM 在运行时试图解析对 someCarMethod()的调用时，它将失败。在 C++中，你会在这里看到著名的 s *egmentation 错误*。问题的根本原因不是第 5 行，而是第 3 行，在那里我们将 not car 实例放入 car 的数组。所以在 Java 中，运行时检查被加入。**

**还要注意，每次使用括号操作符(通过 index，[])访问数组时，还要检查索引是否大于 0 且小于数组的长度(对于空数组，总是会抛出异常)。**

**⁸这是有点问题的，也许 IllegalArgumentException 甚至 NullPointerException 更好。**

**⁹:这是优化，因为如果我们取消检查数组的长度是否为 0，代码仍将返回 0(它不会进入循环，sum 用 0 初始化，这将被返回)，但这将需要更长的路径(这将花费更多的时间)。**

**⁰:如果这是生产代码，我会使用“for-each”迭代。出于演示目的，我使用了显式代码。**

**这个“for-each”特性是在 5.0 版本中添加到 Java 中的。如何生成代码取决于编译器。注意，如果数组为空，那么将抛出 NullPointerException。还要注意，我们获得了一些代码抽象，但我们丢失了一些信息，我们可以访问数组中的每个元素，但我们不知道它们的索引。在我们的例子中，这个信息并不重要。**

**有些 int 不能保证在 int 范围内；大于整数的值。如果我们把 MAX_VALUE 看作 int 值，它就被解释为负值，如果看作 long 值，它就被解释为大正值。**

**从概念上讲，**方法重载**就是[静态多态](https://beginnersbook.com/2013/04/runtime-compile-time-polymorphism/)。你可以在这里阅读[关于这一点的](https://www.scientecheasy.com/2019/02/method-overloading-in-java.html)很好的解释。真正的规则相当复杂，你可以在这里找到它们**

**⁴编译器将检查方法是否被覆盖，否则将产生编译时错误。此注释是在 JDK 5.0 ⁶.中添加的**

**⁵[Java bridge methods explained](http://stas-blogspot.blogspot.com/2010/03/java-bridge-methods-explained.html)是一篇值得一读的好文章。简而言之，它是编译器添加的合成方法。**

***注意*:这个方法不仅可以为另一个方法做调用转移，还可以返回*类型适配*比如强制转换(参见[https://docs . Oracle . com/javase/8/docs/API/Java/lang/class . html # cast-Java . lang . object-](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#cast-java.lang.Object-))或者装箱/拆箱。**

***旁注* : [方法签名中的 Volatile](https://stackoverflow.com/a/21102448/1137529)——当你使用反射或一些反编译工具时，你会遇到这种情况。**

**java 平台总是有各种各样的特别注释机制。例如,`transient`修饰符是一个特别的注释，指示一个字段应该被序列化子系统忽略，而`@deprecated` javadoc 标签是一个特别的注释，指示该方法不应该再被使用。从 5.0 版本开始，该平台有一个通用注释(也称为*元数据*)工具，允许您定义和使用自己的注释类型。该工具由声明注释类型的语法、注释声明的语法、读取注释的 API、注释的类文件表示和注释处理工具(apt)组成。从 5.0 版本的 [Java 开发工具包](https://en.wikipedia.org/wiki/Java_Development_Kit) (JDK)开始，注释在语言本身中变得可用。`[apt](https://en.wikipedia.org/wiki/Annotation_processing_tool)` [工具](https://en.wikipedia.org/wiki/Annotation_processing_tool)为 JDK 版本 5.0 中的编译时注释处理提供了一个临时接口；JSR-269 对此进行了形式化，并在版本 6 中集成到了 [javac](https://en.wikipedia.org/wiki/Javac) 编译器中。**

**⁷Quote: *类型擦除可以被解释为仅在编译时强制类型约束并在运行时丢弃元素类型信息的过程。*[https://www.baeldung.com/java-type-erasure](https://www.baeldung.com/java-type-erasure)**

**更多详细解释请见上面的链接。特别有趣的是用桥⁵方法解决的边界情况。**

***注:*截至 2019 年[瓦尔哈拉项目](https://openjdk.java.net/projects/valhalla/)仍在进行中。**

***注意:* `[JLS8 §4.6](https://docs.oracle.com/javase/specs/jls/se8/html/jls-4.html#jls-4.6)` 在一些非常特殊的情况下，提供一些循环孔，用于在运行时检索泛型类型信息。**

**注意 clazz.getGenericSuperclass()的用法。您可以利用这一点:**

**您可以使用匿名类来捕获泛型类型信息，而不是使用显式类定义。在字节码级别，它实际上是在类定义的签名中捕获的。详见[https://helw . net/2017/11/09/runtime-generics-in-a-erasure-world/](https://helw.net/2017/11/09/runtime-generics-in-an-erasure-world/)。com . Google . gson . reflect . type token 是利用这一事实的标准事实。**

***旁注:*在 JDK 5.0 中，在<类旁边增加了类型层次？>。类型是编译类型构造。在运行时有原始类型(建模为类<？>)、参数化类型、数组类型、类型变量、原语类型。这两者有着非常复杂的关系，Gson 库试图为程序员简化。它还有 API 来动态创建类型的实例。**

**引用:**

> **Java 的类型擦除适用于单个对象，**而不是类**或字段或方法。TypeToken 使用一个匿名的**类来确保它保存泛型类型信息**，而不仅仅是创建一个对象。**

**[https://stack overflow . com/questions/30005110/how-gson-type token-work](https://stackoverflow.com/questions/30005110/how-does-gson-typetoken-work)**

**另一句话**

> **总之，java 语言规范[指定了](https://docs.oracle.com/javase/specs/jls/se8/html/jls-4.html#jls-4.6)参数化类型、嵌套类型、数组类型和类型变量的擦除类型是什么。然后它说“每一个其他类型的擦除是类型本身。”**

**[https://helw . net/2017/11/09/runtime-generics-in-a-erasure-world/](https://helw.net/2017/11/09/runtime-generics-in-an-erasure-world/)**

**基于[https://stackoverflow.com/a/33738910/1137529](https://stackoverflow.com/a/33738910/1137529)⁸
的不变性类型:**

1.  **可变——你应该改变集合(科特林的`MutableList`)**
2.  **readonly——你不应该改变它(Kotlin 的`List`),但是有些东西可以改变(转换成 Mutable，或者从 Java 改变)**
3.  **不可变——没有人能改变它(Guavas 的不可变集合，Scala 的不可变集合)**

**所以在第(2)种情况下，`List`只是一个没有变异方法的接口，但是如果你把它转换成`MutableList`，你可以改变实例。**

**使用 Guava 或 Scala(第三种情况)，任何人都可以安全地更改集合，即使是通过强制转换或其他线程。**

**Kotlin 选择 readonly 是为了直接使用 Java 集合，所以在使用 Java 集合时没有开销或转换。**

***注意* : Kotlin `List`是只读的，不是不可变的。其他调用者(例如 Java)可能会更改列表。Kotlin 调用者可能会转换列表并更改它。没有一成不变的保护。**

**你可以在这里阅读[https://stackabuse.com/javas-object-methods-equals-object/](https://stackabuse.com/javas-object-methods-equals-object/)的简短解释或者直接阅读 [java doc](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#equals-java.lang.Object-) 。**

**⁰ [java.sql.Timestamp](https://docs.oracle.com/javase/8/docs/api/java/sql/Timestamp.html) 有一个 bug (java.sql.Timestamp 继承自 java.util.Date)，它有方法 equals(时间戳)和 not equals(对象)。由于向后兼容性问题，此方法破坏了相对于`java.util.Date.equals(Object)` 的对称性。如果传递的对象确实是 Timestamp 的实例，并且如果传递的对象不是 Timestamp 的实例(例如，它是 java.util.Date ),则返回 false。让我们看看一些代码示例，看看问题出在哪里:**

**注意:java.util.Date 以毫秒为单位存储“日期”。java.sql.Timestamp 具有纳秒精度(为了达到这一精度，它添加了 nanos)。**

**很明显，我们正在用相同的底层值(时间)构造 java.util.Date 和 java.sql.Timestamp。因此，我们期望 equals()将返回 true。**

**实际上，在第 14 行中，我们验证了 d 和 ts 都是 java.util.Date 的实例，展开底层时间值并进行比较。**

**但是在第 15 行，我们得到了意想不到的结果。这里调用 equals(Object)。**

**因为 d 不是 java.sql.Timestamp 的实例，所以它返回 false。**

**我们可以尝试将实现更改为如下形式:**

**这里，如果 ts 不是时间戳的实例，我们将检查委托给 ts 本身。如果是 java.util.Date，则会调用 java.util.Date.equals(Object)。这种实现中问题是，时间放大器的附加场在这种比较中将被忽略。因此，如果我有两个相差不到 1 毫秒的“日期对象”，那么 java.util.Date.equals(Object)将返回 true，这在语义上是错误的。**

**如您所见，equals()方法将 ts 和 d 视为相等，尽管事实上它们在纳秒级别上明显不同(这对时间戳有明显的影响)。同样，这在语义上是错误的。最好打破 equals()对称。**

**引用自[时间戳的 java 文档](https://docs.oracle.com/javase/8/docs/api/java/sql/Timestamp.html):**

> ****注意:**这个类型是一个`java.util.Date`和一个单独的纳秒值的组合。只有整数秒存储在`java.util.Date`组件中。小数秒-纳米-是分开的。当传递一个不是`java.sql.Timestamp`实例的对象时，`Timestamp.equals(Object)`方法从不返回`true`，因为日期的 nanos 部分是未知的。结果，`Timestamp.equals(Object)`方法相对于`java.util.Date.equals(Object)`方法不对称。此外，`hashCode`方法使用底层的`java.util.Date`实现，因此在其计算中不包括 nanos。**
> 
> **由于上面提到的`Timestamp`类和`java.util.Date`类之间的差异，建议代码不要将`Timestamp`值视为`java.util.Date`的实例。`Timestamp`和`java.util.Date`之间的继承关系实际上表示实现继承，而不是类型继承。**

**只有一种方法可以避免上面描述的混乱。*不要重载* equals(Object)方法，只有*重载*它。**

**我看到过一些讨论，把这个警告默认为错误。从 JDK 5.0 版本开始已经过去很久了。这显然破坏了向后兼容性，因此在此基础上拒绝了它。**

**允许 Null，因为 null 是捕获#15-of 的实例。扩展 Car (null 是 Java 中任何类型的实例)。**

**你可以阅读从 Lambdas 到字节码的 Lambda 机制[和 Lambda 表达式的翻译](http://wiki.jvmlangsummit.com/images/1/1e/2011_Goetz_Lambda.pdf)[尤其是你可能对“方法引用捕获”小节感兴趣。](http://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html)**

**JDK-6785114:返回类型推断(15.12.2.8)对自动装箱不起作用类型:增强，组件:规范，受影响版本:7。正如你所看到的，JDK 7 的规范改变了返回类型中对象的使用。你可以在 [JDK-6785112 : Umbrella:泛型方法类型推断的问题](https://bugs.java.com/bugdatabase/view_bug.do?bug_id=6785112)上找到其他的修复方法。如果你使用 JDK 8 和更新版本，这些错误对你来说只有历史意义。**