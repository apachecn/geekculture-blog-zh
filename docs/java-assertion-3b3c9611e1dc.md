# Java 断言

> 原文：<https://medium.com/geekculture/java-assertion-3b3c9611e1dc?source=collection_archive---------1----------------------->

# 介绍

在 JDK 1.4 中，增加的一个特性是`assert`关键字。(下一个 JDK 版本是 1.5，即 as 5.0 所以，这个前泛型 Java)。那时候我很热情。这个特性实际上在 Python 中是有效的。但是在 Java 中，几乎没有人使用它。在这篇文章中，我将描述这个特性，为什么它在 Java 中会失败，以及它在其他语言中是如何工作的。您可以在这里找到完整的描述[https://docs . Oracle . com/javase/8/docs/technotes/guides/language/assert . html](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html)和这里[https://docs . Oracle . com/javase/specs/jls/se14/html/jls-14 . html # jls-14.10](https://docs.oracle.com/javase/specs/jls/se14/html/jls-14.html#jls-14.10)

# 让我们看看具体的例子:

注意，如果 interval ≤0 或 interval> `MAX_REFRESH_RATE`，上述断言将失败。

第 8 行发生了什么？首先表情`interval > 0 && interval <= MAX_REFRESH_RATE`被评价。如果值为真，我们将进入下一行。否则，`java.lang.AssertionError`被构造并抛出，同时`String.valueOf(interval)`被传递给它的构造函数。

**注意:**可以省略第二个参数(`assert interval > 0 && interval <= MAX_REFRESH_RATE`)，此时将调用`java.lang.AssertionError` 的无参构造函数。

在这个例子中，我们不想断言某个方法`precondition`成立。另一种选择是:

注意，这里我们抛出的是`IllegalArgumentException`(这是通常会发生的情况；但是如果我们愿意，我们可以扔出`AssertionError`。此外，信息有点不同，但我们也可以改变它。

你注意到了吗，那个`setRefreshInterval`是私有方法？这是有意的，我稍后将回到这一点。

# 引用

现在，你对断言有了基本的了解，我想给你一个引语:

> *一般问题*
> 
> 既然可以在 Java 编程语言之上编写断言而不需要特殊支持，为什么要提供断言工具呢？
> 
> *尽管特定的实现是可能的，但是它们要么是丑陋的(每个断言都需要一个* `*if*` *语句)，要么是低效的(即使断言被禁用也要评估条件)。此外，每个特定的实现都有自己的启用和禁用断言的方法，这降低了这些实现的实用性，尤其是在现场调试时。由于这些缺点，断言从未成为使用 Java 编程语言的工程师文化的一部分。向平台添加断言支持很有可能纠正这种情况。*
> 
> 相对于库解决方案，为什么这个工具证明了语言改变的合理性？
> 
> *我们认识到语言的改变是一项严肃的工作，不能掉以轻心。考虑了图书馆办法。然而，如果断言被禁用，那么断言的运行时开销可以忽略，这被认为是必不可少的。为了用库实现这一点，程序员被迫将每个断言硬编码为一个* `*if*` *语句。许多程序员不会这样做。他们要么省略 if 语句，性能会受到影响，要么完全忽略该工具。还要注意，断言包含在詹姆斯·高斯林最初的 Java 编程语言规范中。Oak 规范中删除了断言，因为时间限制妨碍了令人满意的设计和实现。*
> 
> 为什么不像 Eiffel 编程语言那样，提供一个带有前置条件、后置条件和类不变量的成熟的契约式设计工具呢？
> 
> *我们考虑过提供这样一个工具，但是我们无法说服自己，如果不对 Java 平台库进行大规模的修改，并且新旧库之间不存在大规模的不一致，就有可能将它移植到 Java 编程语言上。此外，我们不相信这样的工具会保持简单性，而简单性是 Java 编程语言的标志。总的来说，我们得出的结论是，简单的布尔断言工具是一个相当直接的解决方案，而且风险要小得多。值得注意的是，向语言中添加布尔断言功能并不排除在将来的某个时候添加完全成熟的契约式设计功能。*
> 
> *简单断言工具确实支持有限形式的* [*契约式设计风格编程*](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html#usage-conditions) *。* `*assert*` *语句适用于非公共前置条件、后置条件和类不变量检查。公共前提条件检查仍应通过方法内部的检查来执行，这些检查会导致特定的、记录在案的异常，例如* `*IllegalArgumentException*` *和* `*IllegalStateException*` *。*
> 
> ***除了布尔断言，如果断言被禁用，为什么不提供一个类似 assert 的构造来抑制整个代码块的执行？***
> 
> 当复杂的断言被更好地归入单独的方法时，提供这样的构造将鼓励程序员把它们内联。
> 
> …
> 
> ***为什么*** `***AssertionError***` ***是*** `***Error***` ***的子类而不是*** `***RuntimeException***` ***？***
> 
> 这个问题很有争议。专家组对此进行了详细的讨论，得出的结论是 `*Error*` *更适合阻止程序员试图从断言失败中恢复。一般来说，定位断言失败的来源是困难的或不可能的。这种失败表明程序正在“已知空间之外”运行，试图继续执行很可能是有害的。此外，惯例规定方法指定它们可能抛出的大多数运行时异常(带有* `*@throws*` *doc 注释)。在一个方法的规范中包含它可能产生断言失败的环境是没有意义的。这种信息可以被视为实现细节，它可以随着实现和发布的不同而变化。*
> 
> …
> 
> ***为什么不提供一个构造来查询包含类的断言状态？***
> 
> *布尔型 assertsEnabled = false【assertsEnabled = true//* ***故意的副作用！！！*** *//现在 assertsEnabled 被设置为正确的值*

现在，让我们来看看这段引文。

"*虽然特定的实现是可能的[参见上面的例子]，但是它们必然是丑陋的(每个断言需要一个* `*if*` *语句)"*

不完全是。从 JDK 8 开始(引用自 JDK 8.0)，我们在语言中有了`Lambda` 。我们可以将条件包装到`Lambda` ，并将其作为第一个参数传递给`assert`语句。

至于*第二个*参数，可以使用`Lambda` ，它捕获的值将被解释为`String`，并在评估时传递给`AssertionError`构造器。另一个选项，甚至在添加`Lambda` 之前，它就已经使用了类似于`[logback](http://logback.qos.ch/documentation.html)`的设计:

[http://logback.qos.ch/xref/index.html](http://logback.qos.ch/xref/index.html)

在语言中使用`Lambda`之前，使用内部类确实很难看，但在 lambda 中这应该没问题。

"*…或者效率低下(即使断言被禁用也要评估条件)。此外，每个专用实现都有自己的启用和禁用断言的方法…”*

大约三分之一的引用文档和大约一半的`assert`规范是关于何时启用/禁用断言以及我们如何进行精细控制的讨论。理论上，我们只能在特定的包/子包甚至特定的类中启用`assertion`。还讨论了我们如何从编译的`class`-文件中消除这些断言(就像 C++中的 micro 一样，我们可以改变一些变量的值，重新编译代码，所有赋值都将被移除)。我在实践中从未见过这样的用法。我从来没有见过一些开源项目使用这个。所以，所有这些复杂性**从未**用于实践。

我看到的是 2 种使用模式:

*   *默认*一:没有额外的参数添加到`java` —这通常用于生产或阶段。
*   标志`-ea` :使能断言。**断言在所有(非 JDK)类中被启用**。该选项通常在 ide 中启用。它用于在调试模式下本地运行应用程序，但主要用途是**运行单元测试，**无论它们是在 ide 中运行，作为构建的一部分在 maven 中运行，还是在构建服务器上运行。*我在* ***制作*** *中也用过，不过这种用法有争议***见下文。**

***注:***

1.  *我从未见过或使用过`-esa` 标志:启用系统断言。*
2.  *如果你关注 maven 插件，IDE 的采用，你会发现他们只支持`-ea`标志。*
3.  *我使用了以下代码片段:*

> **要求启用断言**
> 
> *某些关键系统的程序员可能希望确保断言在现场没有被禁用。下面的静态初始化习语防止一个类被初始化，如果它的断言已经被禁用…*
> 
> *把这个静态初始化器放在你的类的顶部。*

*我将上面的代码片段放在应用程序初始化时调用的类中。*

*"*……或低效"*(再次)*

**通常是*，第一个布尔表达式非常简单。类似于检查值是否不为空。如果表达式如此简单，那么在*典型的*应用中，求值表达式的开销(当断言被禁用时——因此它无论如何都会被丢弃)是微不足道的。*

*另一句话:*

> **通常，断言中包含的表达式应该没有副作用*s:对表达式求值不应该影响求值完成后可见的任何状态。这条规则的一个例外是，断言可以修改只在其他断言中使用的状态。本文稍后将介绍利用此异常的习语。*

*[https://docs . Oracle . com/javase/8/docs/technotes/guides/language/assert . html](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html)*

*如果第一个布尔表达式是复杂的，它应该仍然是“*没有副作用*”(见上面引用)。这样的用法*不典型。在这种情况下，开销不能被忽略，但是应该问另一个问题:*你能负担得起在生产中花费一些额外的执行时间来捕获 bug 吗？*我们知道调试的最佳环境是生产环境。如果你的答案是肯定的，你会选择加入这样的低效率。顺便提一下，在我过去参与的项目中，我总是坚持在产品中启用断言来运行。**

***推论:***

*让我们回到问答环节:*

> ****相对于库解决方案，为什么该工具证明了语言更改的合理性？****

*作为上述 JDK 8 及以上版本的推论，它可以作为库来实现。*

*JDK 1.4 于 2002 年发布。JDK 1.7 在 2011 年年中发布，几乎时隔 9 年。JDK 1.8(又名 8.0)于 2014 年 3 月发布，时隔 3 年多。JDK 1.7 为什么有趣？因为，当时 [java.util.Objects](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html) 已经发布了。让我们看看它是 JavaDoc:*

> **该类由* `*static*` *对对象进行操作或在操作前检查某些条件的实用方法组成。这些实用程序包括* `*null*` *-safe 或* `*null*` *-tolerant 方法，用于计算对象的哈希代码、返回对象的字符串、比较两个对象以及检查索引或子范围值是否超出界限。**
> 
> **API 注:*`[*checkIndex(int, int)*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#checkIndex(int,int))`*`[*checkFromToIndex(int, int, int)*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#checkFromToIndex(int,int,int))`*`[*checkFromIndexSize(int, int, int)*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#checkFromIndexSize(int,int,int))`*等静态方法是为了方便检查指标和子范围对应的值是否越界而提供的。这些静态方法的变体支持运行时异常的定制，以及相应的异常详细信息，当值超出界限时抛出。这种方法接受一个函数接口参数，即* `*BiFunction*` *的实例，它将越界值映射到运行时异常。在将此类方法与 lambda 表达式、方法引用或捕获值的类的参数结合使用时，应该小心。在这种情况下，与功能接口分配相关的捕获成本可能会超过检查边界的成本。****
> 
> ****自:1.7
> ……****
> 
> ****公共静态<T>T require nonnull(T obj)****
> 
> ****检查指定的对象引用不是* `*null*` *。该方法主要用于在方法和构造函数中进行参数验证，如下所示:****
> 
> ****public Foo(Bar Bar){ this . Bar = objects . require nonnull(Bar)；}****
> 
> ****类型参数:****
> 
> ***`*T*` *-参考的类型****
> 
> ****参数:****
> 
> ***`*obj*` *-检查无效性的对象引用****
> 
> ****返回:* `*obj*` *如果不是* `*null*`***
> 
> ****抛出:*`[*NullPointerException*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/lang/NullPointerException.html)`*——如果* `*obj*` *是* `*null
> ...*`***
> 
> ****公共静态< T > T requireNonNull (T obj，* [*供应商*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/function/Supplier.html)*<*[*字符串*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/lang/String.html) *>消息供应商)****
> 
> ****检查指定的对象引用不是* `*null*` *，如果是则抛出自定义的* `[*NullPointerException*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/lang/NullPointerException.html)` *。****
> 
> ****与方法* `[*requireNonNull(Object, String)*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#requireNonNull(T,java.lang.String))` *不同，该方法允许延迟创建消息，直到进行空值检查之后。虽然这在非空的情况下可能会带来性能优势，但是在决定调用此方法时，应该注意创建消息提供者的成本要小于直接创建字符串消息的成本。****
> 
> ****类型参数:* `*T*` *-引用的类型
> 参数:* `*obj*` *-检查无效性的对象引用* `*messageSupplier*` *-抛出* `*NullPointerException*` *事件中使用的详细消息的供应商
> 返回:* `*obj*` *如果不是* `*null*` *抛出:* `[*NullPointerException*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/lang/NullPointerException.html)` *-如果****
> 
> ****公共静态 int checkIndex (int index，int length)****
> 
> ****检查* `*index*` *是否在* `*0*` *(含)到* `*length*` *(不含)的范围内。****
> 
> ****如果以下任何一个不等式成立，则* `*index*` *被定义为出界:****
> 
> ***`*index < 0*`***
> 
> ***`*index >= length*`***
> 
> ***`*length < 0*` *，这是从前不等式*中隐含的***
> 
> ****参数:* `*index*` *-指标* `*length*` *-范围*的上限(不含)***
> 
> ****返回:* `*index*` *是否在范围*的界限内***
> 
> ****抛出:*`[*IndexOutOfBoundsException*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/lang/IndexOutOfBoundsException.html)`*——如果* `*index*` *出界****
> 
> ****自:9****

***Objects.requireNonNull ( 或其变体)在开源项目中被广泛使用。主要优点——它可以用在**公共**方法中。它也可以内联使用(它返回非空值)。***

***[*objects . require nonnull(T，Java . util . function . supplier)*](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#requireNonNull(T,java.util.function.Supplier))*(或者是我在这里省略的它的变体)可以被使用，以便*只在需要的时候构造错误消息。*****

***[JDK 9 新增 Objects.checkIndex(int，int)](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#checkIndex(int,int)) 。***

*****所有这些方法都清楚地表明** `**assert**` **设施出现故障。*****

***让我们回到上面的问答。***

> ****还要注意断言包含在詹姆斯·高斯林的 Java 编程语言的原始规范中。Oak 规范中删除了断言，因为时间限制妨碍了令人满意的设计和实现。****

***JDK 1.0 于 1996 年 1 月发布。JDK 1.4 于 2002 年 2 月发布。需要 6 年吗？[JSR 335:JavaTM 编程语言的 Lambda 表达式](https://www.jcp.org/en/jsr/detail?id=335)花了 4 年时间，这个特性花了 6 年？***

***让我们回到上面的问答。***

> ******为什么不像 Eiffel 编程语言那样，提供一个带有前置条件、后置条件和类不变量的成熟的契约式设计工具呢？
> …*** *值得注意的是，向语言中添加布尔断言功能并不排除在将来的某个时候添加一个成熟的按合同设计功能。****
> 
> ****简单的断言工具确实支持有限形式的* [*契约式设计风格的编程*](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html#usage-conditions) *。* `*assert*` *语句适用于非公共前置条件、后置条件和类不变量检查。公共前提条件检查仍然应该通过方法内部的检查来执行，这些检查会导致特定的、记录在案的异常，例如* `*IllegalArgumentException*` *和* `*IllegalStateException*` *。****

***好吧，Java 再也没有回到*完全的契约式设计，*尽管很明显*布尔断言工具*已经失败了(没有被广泛使用， [java.util.Objects](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html) )。***

***事实上，科特林决定增加合同设计。参见[这篇](https://www.baeldung.com/kotlin-contracts)文章或者[这篇](https://kotlinlang.org/docs/reference/whatsnew13.html#contracts)官方页面。在 Kotlin 1.4 中，DSL 仍然是试验性的，这可以被看作是很难设计和实现成熟的按合同设计设施的标志。***

***至于前置条件、后置条件和类不变量，我将在下面回到这个问题。我稍后还会进行公开和私下的讨论***

***让我们回到上面的问答。***

> ******除了布尔断言，如果断言被禁用，为什么不提供一个类似 assert 的构造来抑制整个代码块的执行？******
> 
> ***当复杂的断言被更好地归入单独的方法时，提供这样的构造将鼓励程序员将它们内联。***

***语言中有 Lambda 表达式使得这个语句很奇怪。我可以在 Lambda 中放置任意复杂的“代码块”并调用它。只有当断言被启用时，我才能做到这一点，最简单的方法是使用提议的习语(见 ***为什么不提供一个构造来查询包含类的断言状态？*以上**)。***

***让我们回到上面的问答。***

> ******为什么*** `***AssertionError***` ***是*** `***Error***` ***的子类而不是*** `***RuntimeException***` ***？******
> 
> ***这个问题很有争议。***

***这其实是很有意思的一点。我将在单独的文章中展开它。***

***引用:***

> ******Do* not *在*** `***public***` ***方法中使用断言进行论证检查。******
> 
> ****参数检查通常是方法的已发布规范(或合同)的一部分，无论启用还是禁用断言，都必须遵守这些规范。使用断言进行参数检查的另一个问题是，错误的参数应该导致适当的运行时异常(如* `*IllegalArgumentException*` *、* `*IndexOutOfBoundsException*` *或* `*NullPointerException*` *)。断言失败不会抛出适当的异常。****

*****这一点很难与另一个开发者沟通，我认为这是这个“*简单布尔断言工具”*失败的原因。*****

***我通常会被问到两个不同的问题:***

1.  ***为什么我用来检查方法前提条件的工具依赖于方法的*可见性*？***
2.  ***无论如何`private`方法应该对前提条件进行检查吗？***

***让我们从第二点开始。许多程序员说，“嘿，你知道这个方法是如何被调用的，为什么和在哪里。为什么你的私有方法要担心空值[或者任何其他的先决条件]？这是“永远不会发生”的支票。他们基本上是说，`private`方法没有定义契约，它们在同一个类中使用，`private`方法和`calling site`都在你的控制之下，何必麻烦呢？从纯理论的角度来看，这是正确的。但是，实用的答案是: **bug。如果你的代码有一些 bug 怎么办？您正在使用意外的参数调用您的`private`方法。您希望尽快得到关于它的指示，因为这将使追踪 bug 的根源变得更加容易。*****

***作为使用 assert 语句的替代方法，您可以编写单元测试。是的，单元测试为`private`方法(我知道，这是一个有争议的话题)。***

*****边注:**你可以使用核心反射 API 来调用这个`private`方法，或者你可以将它的可见性改为 package-private，最好在它上面加上一些`@VisibleForTesting`注释。主要缺点:就是*耗时*。但是如果你认为这个`private`方法可以在以后的其他地方使用(或者通过复制&粘贴或者通过适当的重构使它成为`public`，这可能是值得的。***

***现在，让我们回到第一点。为什么来自(`public, package-private, protected, private`)的方法的可见性会以任何方式影响我如何验证前提条件。如果设计决策是检查前提条件(可能是，例如，这是任务关键部分，我们应该在这里使用防御性编程),那么进行这种检查的*工具*应该是相同的，不管有什么可见性方法。***

***如果我有一些检查了前提条件的`private`方法，并且我对这个方法进行了覆盖良好的单元测试，我决定让这个方法`public`只要我的新调用使用了前提条件检查中覆盖的参数(在这个例子中，我有单元测试来验证这一点)**我就不必对代码做任何更改。*****

***此外，让我们看看另一种语言。例如，在 Python 中。这是从我以前的文章[Python 中的合作多重继承:实践](/@alex_ber/cooperative-multiple-inheritance-in-python-practice-60e3ac5f91cc?source=your_stories_page---------------------------)中引用的另一篇文章:***

***Based on [http://code.activestate.com/recipes/577720-how-to-use-super-effectively/](http://code.activestate.com/recipes/577720-how-to-use-super-effectively/)***

***这里有趣的是根类:***

```
***class Root(object):
    def draw(self):
        # the delegation chain stops here
        #assert not hasattr(super(), 'draw')
        assert not any('draw' in B.__dict__ for B in type(self).__mro__[1:])***
```

***这个类检查根类是否被放置在继承层次中的“写”位置，它检查他是否在`object`之前。`assert`使用了设备，这完全没问题。***

*****边注:**在 Python 中，`private`方法是以`_`为前缀的方法。这纯粹是命名约定，不像在 c 语言中那样由 Python 强制执行。对于“真实”“`private`”方法也有[名称处理算法](https://www.python.org/dev/peps/pep-0008/)，但这与`draw()`方法无关。尽管如此，这里的`draw()`是`public`方法，而`assert`工具的使用完全没问题。***

***那么，为什么只有 Java 对方法可见性有这种奇怪的限制呢？我不知道这个问题的好答案…***

***还有一点，为什么如果我决定用`assert` 工具检查前提条件，我只能抛出`AssertionError`？也许，`IllegalArgumentException` 对我来说再合适不过了。可能，我预计在不久的将来这种方法会变成`public`，我希望使用`IllegalArgumentException`。为什么我现在不能用`assert`工具用`private`方法来做？这个问题的答案是保持`assert`设备简单…***

***让我们回顾一下 Java 团队推荐使用`assert`工具的地方。***

# ***前提***

***我们已经在上面看到了前提条件的例子。我想重复一遍，*通常是*，这样的前置条件检查很简单，大部分从 JDK 1.7 开始在 [java.util.Objects](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html) 中就有了。在不同的开源项目中还有另一个实用程序类，我不会在这里介绍。注意，它们可以在`private`和`public`方法`.`中使用***

***让我们看看更复杂的例子。***

> ****锁定状态前提条件****
> 
> ****为多线程使用而设计的类通常具有非公共方法，这些方法带有与是否持有某个锁相关的前提条件。比如，类似这样的事情并不少见:****

> ****在* `*Thread*` *类中增加了一个名为* `*holdsLock*` *的静态方法，用于测试当前线程是否持有指定对象的锁。此方法可与* `*assert*` *语句结合使用，以补充描述锁定状态前提条件的注释，如下例所示:****

*****注:*****

1.  ***你可以在`[Throwable.printEnclosedStackTrace()](https://github.com/openjdk/jdk14/blob/master/src/java.base/share/classes/java/lang/Throwable.java#L691)`的源代码中找到这样的实际用法。***
2.  ***这种先决条件检查至少不能在单元测试中轻松模拟。***
3.  ***另一方面，这种特殊的`Thread.holdsLock()`用法很少见，我的第三方库中只有几个这样的实例。***
4.  ***在更一般情况下，这种复杂的前提条件检查编写起来很复杂(要正确编写它们就更复杂了:-)。实际上，我从来没有这样做过。偶尔，我会验证是否调用了某个特定的方法(参见`[Mockito.verify()](https://github.com/mockito/mockito/blob/b6ae6cf12b93ef9445e524224375aab1eb76129d/src/main/java/org/mockito/Mockito.java#L2401)`)或者进行了一些额外的测试(例如压力测试)。***

# ***后置条件***

> ****例如，下面的公共方法使用一个* `*assert*` *语句来检查一个 post 条件:****

***嗯，我在实践中从未见过这样的用法。这里单元测试更适合。无论如何，你都应该拥有它们。这种计算量很大，所以您应该在生产中将其关闭(否则，您的性能会受到严重影响)。因此，单元测试比这里的`assert` 工具更合适。***

*****注意:**有一些关于如何“在执行计算之前保存一些数据以检查后置条件”的讨论。它使用内部类。我从未见过这样的用法，所以我不会在这里提供它。***

# ***控制流不变量***

> ****…:* ***将断言放置在您认为不会到达的任何位置*** *。要使用的断言语句是:****
> 
> ***`*assert false;*`***
> 
> ****举个例子，假设你有一个看起来像这样的方法:****

> ****替换最后的注释，这样代码现在显示为:****

> ******注:*** *慎用此术。如果按照 Java 语言规范*中的定义，一个语句是不可到达的，那么如果你试图断言它是不可到达的，你将得到一个编译时错误。同样，一个可接受的替代方法是简单地抛出一个`AssertionError`。***

***为什么这么麻烦，如果我可以抛出一些异常？可以是这里所说的`*AssertionError*` ，也可以是例如`IllegalStateException`。这并不重要，因为无论如何都不应该执行这个异常。***

***[https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/util/ReflectionUtils.java](https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/util/ReflectionUtils.java)***

***这里，你可以看到`IllegalStateException`是为了取悦编译器而抛出的。这不仅是“可接受的选择”，这是在现实中使用的方式。***

# ***内部不变量***

> ***在断言可用之前，许多程序员使用注释来表明他们对程序行为的假设。例如，你可能会写这样的东西来解释你对一个多向 if 语句中的 `*else*` *子句的假设:****

> ****你现在应该* ***使用断言，只要你已经写了断言不变量*** *的注释。例如，您应该像这样重写前面的 if 语句:****

> ***顺便注意，如果`i`为负，则上述示例中的断言可能会失败，因为`%`运算符不是真正的*模数*运算符，而是计算可能为负的*余数*。***

***这的确很合身。这个`assert`语句服务器用于运行时验证(当断言被启用时)和意图文档。它是**可读的**，并且它的性能影响很小([欧几里德算法](https://en.wikipedia.org/wiki/Euclidean_algorithm)非常快，无论你是应用它 2 次还是 3 次都没有关系)。***

***然而，**备选方案是具有良好代码覆盖率的单元测试。**你应该有涵盖这个 else 语句的单元测试。***

# ***内部不变量(开关)***

> ****另一个很好的断言候选是没有* `*default*` *格的* `*switch*` *语句。一个* `*default*` *案例的缺席通常表明程序员相信其中一个案例将总是被执行。假设一个特定的变量将有一个小数量的值是一个不变量，应该用一个断言来检查。例如，假设下面的* `*switch*` *语句出现在一个处理扑克牌的程序中:****

> ****它可能表示一个假设，即* `*suit*` *变量只有四个值中的一个。为了测试这个假设，您应该添加下面的默认案例:****

> ****如果* `*suit*` *变量取另一个值并且断言被启用，那么断言将失败并抛出* `*AssertionError*` *。****
> 
> ****可接受的替代方案是:****

> ****即使断言被禁用，这种替代方案也能提供保护，但额外的保护不会增加成本**:*`*throw*`*语句不会执行，除非程序失败。此外，在* `*assert*` *语句不合法的情况下，替代语句是合法的。如果封闭方法返回值，则* `*switch*` *语句中的每个 case 都包含一个* `*return*` *语句，并且没有* `*return*` *语句跟在* `*switch*` *语句之后，则添加带有断言的默认 case 会导致语法错误。(如果没有匹配的大小写并且断言被禁用，则该方法将返回无值)。****

***嗯，我对此有多种意见。***

***先说为什么扔`AssestionError`只是「可接受的替代方案」？为什么这不是默认选择？当在 JDK 1.5(又名 5.0)中添加了`enum`的时候，我已经开始在`switch`中使用它们了——如上所述，我在`IllegalStateException` 中使用了`default`子句(后来，我将其改为`AssestionError`)。如前所述，即使断言被禁用，也没有额外的成本。***

*****注:*****

1.  ***有趣的是，Oracle 并没有遵循本文档的建议，而是在指南示例中使用了`IllegalStateException`。例如参见 [Java 语言更新开关表达式](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html)。***
2.  ***“在`assert`语句不合法的某些情况下，替代方案是合法的”这种情况发生是因为 switch 是`statement`而不是`expression`。实际上`switch expressions`最初是在 Java 12 中引入的，现在仍然是 Java 14 中的[预览功能，但是使用这个，将解决这个问题。](/swlh/keeping-pace-with-whats-new-in-java-14-5fc6232defab)***

***引用自 [Java 语言更新开关表达式](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html):***

> *****穷尽性 *****
> 
> ****与* `*switch*` *语句不同，* `*switch*` *表达式的用例必须是穷举的，这意味着对于所有可能的值，必须有一个匹配的开关标签。因此，* `*switch*` *表达式通常需要一个* `*default*` *子句。但是，对于覆盖所有已知常数的* `*enum*` `*switch*` *表达式，编译器会插入一个隐式的* `*default*` *子句。****

***[https://docs . Oracle . com/en/Java/javase/14/language/switch-expressions . html](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html)***

***引自 JEP 325:***

> ****例中的一个* `*switch*` *表达式必须详尽无遗；对于任何可能的值，都必须有一个匹配的开关标签。实际上，这通常意味着需要一个* `*default*` *子句；然而，在覆盖所有已知情况的* `*enum*` `*switch*` *表达式的情况下(最终，* `*switch*` *表达式覆盖密封类型)* **，编译器可以插入一个** `default`子句，指示`enum`定义在编译时和运行时之间已经改变。(这是今天开发人员手工做的，*但是让编译器插入它不仅干扰性小，而且可能比手工编写的错误消息* 更具描述性的错误消息*。)****

***[https://openjdk.java.net/jeps/325](https://openjdk.java.net/jeps/325)***

***所以，Oracle 在`enum` `switch`表达式中说“不要用`default`子句费心”，编译器会 a)隐式地添加它，b)它会抛出一些异常，c)它会有描述性的错误消息。***

***综上所述，**即使你使用经典开关，只要用** `**default**` **子句就可以了。你*可以*用** `**AssertionError**` **来做这个，但是你不一定要。如果你有** `**enum**` `**switch**` **表达式，考虑使用增强的开关表达式时，它将被释放。*****

# ***类别不变量***

> ****类不变式是一种内部不变式，它在任何时候都适用于一个类的每个实例，除非一个实例从一种一致状态转换到另一种一致状态。类不变量可以指定多个属性之间的关系，并且在任何方法完成之前和之后都应该是真的。例如，假设您实现了某种平衡的树形数据结构。一个类不变量可能是树是平衡的和适当排序的。****
> 
> ****断言机制不强制任何特定的风格来检查不变量。不过，有时将检查所需约束的表达式组合成一个可由断言调用的内部方法会很方便。继续平衡树的例子，实现一个私有方法来检查树是否按照数据结构的指示平衡可能是合适的:****

> ****因为该方法在任何方法完成之前和之后检查应该为真的约束，所以每个公共方法和构造函数在其返回之前都应该包含以下行:****
> 
> ***`*assert balanced();*`***
> 
> ****除非数据结构由本地方法实现，否则通常没有必要在每个公共方法的开头放置类似的检查。在这种情况下，内存损坏错误可能会在方法调用之间损坏“本机对等”数据结构。在这种方法的开头断言的失败将指示这种存储器损坏已经发生。类似地，在其状态可被其他类修改的类中，在方法的头部包含类不变检查可能是明智的。(更好的是，设计类，使它们的状态对其他类不直接可见！)****

***嗯， ***类不变量*是证明代码正确工作的好工具。我从未写过类似于** `**balanced()**` **的方法，也从未在任何数据结构的实现中见过它们。因此，理论上，这是有效的用例，实际上没有人这样做。*****

# ***摘要***

***正如我们在 Python `assert`中看到的，工具工作正常。在 Java 中，甚至 Oracle(几乎)都放弃了这个特性。在它的[新指南示例](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html)上，它使用了`IllegalStateException`(甚至没有`AssertionError`！)，它[修补编译器](https://openjdk.java.net/jeps/325)来隐式添加`default`闭包，该闭包将在`enum` `switch`表达式中抛出(未指定)异常。甚至在此之前，它添加了实用程序类 [java.util.Objects](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html) ,检查一些也可以在`public`方法中使用的前提条件。***

*   ***对于*简单的前提条件检查* [java.util.Objects](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html) (或类似的)应该是比使用`assert`工具更好的选择。***
*   ***对于*复杂的前提条件检查*(如`Thread.holdsLock()` ) `assert`设施是一个有效的选择，但实际上这种前提条件检查很少实施。使用 mocking 框架(例如 JUnit+Mockitor)进行单元测试是另一种选择。***
*   ***对于*后置条件，检查*后置条件*的*单元测试更合适。***
*   **对于*类不变量来说，它从来没有在实践中使用过。***
*   **对于*控制流组件*，可以抛出异常，使用示例见 [ReflectionUtils](https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/util/ReflectionUtils.java) 。**
*   **对于*内部不变量*——无论何时，只要你想写一个断言不变量的注释，就使用断言。举个例子，**

**这的确很合适。它是可读的。**

**然而，另一种选择是具有良好代码覆盖率的单元测试。您应该有涵盖该 else 语句的单元测试。**

**如果这个特性是在 JDK 8 中实现的，那么它是作为库方法实现的。**

**正如我们在 Python `assert`中看到的，工具工作正常。我认为，这个语言特性在 Java 中失败的原因仅限于`non-public` 方法。此外，这个功能被过度设计了，很多注意力都放在了如何有选择地禁用`asserts`上。如果你关注 maven 插件，IDE 采用，你会发现他们只支持开箱即用的`-ea`标志。**

**也可以认为使`AssertionError`延伸`Error`而不是`RuntimeException`是设计错误。然而，我不认为这对这个特性有任何显著的影响。**