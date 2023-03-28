# Java 异常层次结构

> 原文：<https://medium.com/geekculture/java-exception-hierarchy-f6aef08ab9b?source=collection_archive---------0----------------------->

如何使用异常

# 介绍

关于 Java 中的异常，你首先要了解的事情之一是，它们有两种类型——*检查异常*和*未检查异常*。

任何“异常”最终都是从`java.lang.Throwable`继承而来。它有两个直接后代— `java.lang.Error`和`java.lang.Exception`。`java.lang.RuntimeException`是`java.lang.Exception`的直系后裔。

*未检查的异常*是继承自`java.lang.RuntimeException`或`java.lang.Error.`的异常(可能不是立即继承)

*被检查的异常*是从`java.lang.Throwable`继承而来(可能不是立即继承)`java.lang.RuntimeException` 和**而不是**以及**而不是`java.lang.Error`继承而来的异常。*非正式的*，这些是继承自`java.lang.Exception`的异常，而不是继承自`java.lang.RuntimeException`。**

检查异常在新设计的 API 中没有使用，甚至在 JDK 内部也没有使用。

> 这被认为是 Java 语言最大的设计错误。

了解你在哪一层有例外是至关重要的。待遇会不一样。

在 JEE 世界中，有两种类型的异常— *系统异常*和应用程序异常。*系统异常*通常是致命的，它们与程序运行的环境有关。例如，DB 关闭或发生了`OutOfMemoryError`。基本上，这意味着出错的不是应用程序代码，而是超出了应用程序的范围。*应用程序异常*是由应用程序产生的异常。您的应用程序完全负责处理它。我将在下面简要地谈一谈它们。

在 web 服务世界(尤其是微服务世界)中，我们应该讨论将要返回给调用者的异常以及发生在某个内部层的异常。

至于传播到调用者的 Java 异常，一般来说，它应该被转换成符合您用来与调用者通信的协议。例如，如果您使用 Spring Boot 编写 REST 服务，您应该定义`@ExceptionHandler`——[经典方式](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc-ann-exceptionhandler)或[反应流方式](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web-reactive.html#webflux-ann-controller-exceptions)。您应该将您的异常转换成一些结构化数据(通常是 JavaBean ),这些数据将对异常中的所有必要信息进行编码。框架将把这种结构化数据转换成适当的数据格式(例如 JSON 或 Protobuf ),然后发送给调用者。此外，如有必要，应该使用适当的 HTTP 响应状态。

所有的异常大致可以分为以下几类:*非法输入*、*违反了一些基本的业务规则*、*系统级问题*。

# 输入验证

不管是另一个 web 服务调用您还是实际用户填写表单，您都会被调用，您的 web 服务应该做的第一件事就是验证输入。

如果有多个参数，最好的策略是检查所有的参数，并返回封装所有错误参数的异常。

或者，我们可以在发现第一个错误参数时停止输入验证。

显然，前一种策略更好，但有时您的输入中有复杂的关系，如果基本上出了问题，您不想继续进行输入验证。

在 JEE 世界中，这是*应用例外*。

是应该*勾选*还是*未勾选*异常？对于第一层来说，其实并不重要，因为反正会在`@ExceptionHandler`转换。

假设您的 web 服务调用了另一个类中的某个公共方法。可能是服务层，POJO 或者某个实用类。显然，这样的类也应该验证输入。*前置条件*的这部分检查。JDK 内置了一些简单验证的函数，比如[objects . require nonnull()](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#requireNonNull(T))或者[objects . checkfromtoindex()](https://download.java.net/java/GA/jdk14/docs/api/java.base/java/util/Objects.html#checkFromToIndex(int,int,int))。

为了简单起见，让我们考虑参数不为空的验证。*在这种情况下*你应该抛出什么异常？我的观点是，你应该通过这些检查来“保护”你的代码。

另一种选择是不检查并期望您的代码在任意点失败。我在几个项目中见过这种方法。理由如下:*何苦呢——我可以节省不输入这段代码的时间*。无论如何，这最终会以`NullPointerException`失败。主要的缺点是——你的“另一个类”可能会调用另一个类，并且调用链可能是嵌套很深的。你会在某个第三方库中看到这个`NullPointerException`,这需要你花时间去调查根本原因。在更复杂的场景中，如果*的前提条件*更复杂，您甚至可能会返回错误的答案。**最佳实践是尽快验证前提条件。缺点——你花时间进行验证，而验证很可能永远不会失败(你“浪费了你的时间”)。**

这是 JEE 世界的*系统异常*。是“意外的例外”。在许多情况下，它会指出是对 API 的不正确使用还是代码中的错误。如果是 bug，显然与应用程序无关，您应该尽快了解以便修复它。**如果是 API** 的不正确使用，这意味着问题超出了你的模块的责任，这是“环境”问题，即使“环境”只是你的应用程序的另一部分。此类异常应明确 ***未选中一个*** *，*因为是“意外异常”。

所以，第一层的输入验证是*应用异常*(既可以是*选中的*也可以是*未选中的*异常，反正是转换过来的)，其他层是*系统异常*(和*未选中的*一)。

# *违反了一些基本的商业规则*

因此，我们已经成功地通过了*前提条件*检查，包括*输入验证*，并且知道我们正在某个内层制作某个业务逻辑。突然，我们发现一些基本的商业规则被违反了。例如，客户在数据库中不存在。这种情况下我们该怎么办？

在 JEE 世界这个*应用例外。在 JEE，这是你合同的一部分。你的客户应该知道如何应对违反商业规则的情况，而违反商业规则的主要可能性就在你的沟通合同中。*

在 Spring 世界中这将是 ***未选中*** 的异常(Spring 无论如何不要使用*已选中*的异常)，首先。通常，它不会在您的服务层生成，而是在 DAL 层生成。

**注:**

1.  我假设 *DAL 层没有放在单独的微服务*里。如果你为 DAL 层使用单独的微服务，你可以找到另一个例子，这并不重要。
2.  如果您违反了业务规则，这显然应该在您的方法的契约中进行编码。但是 Spring 不使用*为此检查*异常。

理想情况下，您应该在服务层(或第一层)捕捉这样的异常，并将其转换为对您的客户端有意义的内容。

在实践中，我见过这样的项目，向最终用户显示的消息是在发生*应用程序异常*的地方生成的，并放在异常对象本身或某个“全局消息容器”中。

如果我们调用其他方法，却得到了*应用程序异常，该怎么办？*如果这是*未检查的*异常，它很可能会向上传播到堆栈。当然，即使*未选中*，也可以捕捉到这样的*应用异常*。我在生产中遇到这样的异常后，已经用实践添加了这样的捕获；将 catch 子句放在第一位要好得多——只需阅读 javadoc(或者源代码，如果 javadoc 缺失或不完整的话)。

好了，你已经发现了*应用程序异常，*你该怎么办？嗯，因为是*应用异常*你应该知道该怎么做。例如，您可能有这样的业务规则，当您在数据库中寻找某个客户，但没有找到时，您应该创建它，也许用某个标志，所以您搜索客户，得到的*应用程序异常*表明没有找到，所以您用搜索数据(也许用某个标志)创建新客户，将他保存到数据库并继续您的业务流程。这个*可能*完全没问题。

通常，您会将*应用程序异常*返回给调用者，可能会打包到其他异常中。

另一个选项是**将这个异常重新打包成另一个**。您可以捕获一些异常，分析它，然后再抛出另一个异常。
例如，请参见[sqlstatesqlexception translator](https://github.com/spring-projects/spring-framework/blob/master/spring-jdbc/src/main/java/org/springframework/jdbc/support/SQLStateSQLExceptionTranslator.java)和[sqlerrorcodesqlexception translator](https://github.com/spring-projects/spring-framework/blob/master/spring-jdbc/src/main/java/org/springframework/jdbc/support/SQLErrorCodeSQLExceptionTranslator.java)。

**注:**

1.  这些是从`java.sql.SqlException`到 [DataAccessException](https://github.com/spring-projects/spring-framework/blob/master/spring-tx/src/main/java/org/springframework/dao/DataAccessException.java) 的翻译器(详见 javadoc)。
2.  这个翻译机制是`Spring`框架的一部分。这些都是*应用*从`Spring`的角度来看。

不过，这不是一个有代表性的例子。通常，您将只分析被捕获异常的类型和消息，并抛出另一个具有不同消息和类型的异常。通常，您只需将这个捕获的异常包装到另一个异常中，并将捕获的异常作为原因传递。

# 系统异常

系统异常通常是致命的，它们与程序运行的环境有关。例如，DB 关闭或发生了`OutOfMemoryError` 。基本上，这意味着出错的不是应用程序代码，而是超出了应用程序的范围。

通常它们是由 JVM 自己生成的，像`OutOfMemoryError`或`IllegalAccessException` (你知道还有`IllegalAccessError`吗？:-))或一些第三方 API，如 DB 驱动程序。

如果你有`Error`，你通常无事可做，你只想让 JVM 崩溃(这就是为什么你的`catch(Throwable)`使用应该非常有限)。因此，通常情况下，这样的`Error`会将它向上传播到栈中的`main()`函数，甚至更远。

除了`Error`之外，通常*系统异常*会被 ***解除*** 之一。然而，存在一些 API 将一些*系统异常*定义为*检查的*异常。一般来说，这是此类 API 的糟糕设计决策。在这种情况下，你应该如何对待他们？

1.  让它向上传播到堆栈，最终使 JVM 崩溃，就像我们对`Error` s 所做的那样。也许，在向上传播的过程中，您需要将它包装在某个`RuntimeException` 中(或者使用一些肮脏的技巧来填满编译器——这是不推荐的)。
2.  让将它传播到第一级，记录它，并向调用者返回一些一般的错误消息。对于所谓的“意外例外”(见上文)，这通常也是最佳策略。
3.  将它翻译成对您的应用程序更方便的异常(见上面的例子)。
4.  忽略异常，等待超时时间，然后重试或制定其他恢复策略。
    如果您正在使用一些低级 API，或者您正在调用另一个 web 服务，并且您有一些与网络相关的问题将在短期内解决(一些节点暂时断开连接，或者一些更改已经完成，并且它们正在传播网络；在所有内容都是最新的之后，它就可以工作了——例如，DNS 名称更改或 SSL 证书更新。

这意味着如果你不得不做这样的事情，你就只能使用低级别的 API。

# 带有链式异常的代码示例

**注意:**所有示例在 JDK 8 中都能正常工作。在 JDK 9 和更高版本中，这段代码在类路径中运行时是有效的。详见我关于 [Java 平台模块系统](/@alex_ber/java-platform-module-system-953cc88658fb)的文章。

我想给你提供一些明确的例子，演示异常*链接*(当我们将异常包装到另一个异常时，但是我们使用原始异常作为原因)。

**注意:**使用*链式异常*是很久以前的标准做法。它是在 JDK 1.4 版本中添加的。其中一个动机是 `java.sql.SQLException`和`java.rmi.RemoteException`以及支持这个链接工具的类似类，但不是以同样的方式。所以，这个机制是统一的，任何`Throwable`都可以使用。

**例 1。**

[https://medium . com/@ Alex _ ber/explaining-invokedynamic-number-multiply-almost-complete-example-part-iv-50b 211 FD 5702](/@alex_ber/explaining-invokedynamic-number-multiplication-almost-complete-example-part-iv-50b211fd5702)

我们正在调用一些方法(它是在 JDK 1.8，也就是 JDK 8.0 中添加的)，如果结果溢出了一个`int`，它会抛出*未检查的* `ArithmeticException`。作为这个方法的调用者，我们知道我们可以得到`ArithmeticException`，如果这将发生，我们将使用一些替代策略来完成计算。它慢得多，占用更多内存，在大多数情况下，第一次尝试会成功，但它不会溢出。更多详情请见上面的链接。

**例 2:**

[https://medium . com/@ Alex _ ber/explaining-invokedynamic-number-multiply-almost-complete-example-part-iv-50b 211 FD 5702](/@alex_ber/explaining-invokedynamic-number-multiplication-almost-complete-example-part-iv-50b211fd5702)

这里，我们使用`MethodHandles` API 来抛出静态初始化块中的**检查过的**异常。**已检查的**异常无法从静态块中传播出去(实际上是编译错误)，所以我们必须将它们包装在一些**未检查的**异常中。注意，我们将`MethodHandles` API 作为原因来传递，以提高我们稍后理解最初是什么导致了`InternalError`被抛出的能力。

**例 3:**

> *...* `*createMethodHandle*` *大致相当于:*

[https://medium.com/swlh/java-assertion-3b3c9611e1dc](/@alex_ber/explaining-invokedynamic-toy-example-part-ii-674314cfb5a8)

我想重写原来的`*createMethodHandle*` **保留它的合同。**例外条款是合同的组成部分。具体来说，我不想在例外条款中增加新的例外。所以，我把这个新的异常包装到一些现有的异常中。这也是有意义的，我可以说，如果我得到了`MethodType,`中的`NoSuchFieldException`到`java.lang.reflect.Field`，这是`IllegalAccessException`，如果我得到了`SecurityException` ，那么它也是`IllegalAccessException`。这是异常*翻译*的例子(上面有一个到 fancier 的链接)。

注意，我坚持将`SecurityException`或`NoSuchFieldException` 作为原因传递给新生成的`IllegalAccessException`。由于历史原因，它没有将*原因*作为其参数之一的重载构造函数，所以我使用了 JDK 1.4 中专门为这种情况设计的`initCause()`构造。当然，我可以选择更方便的**未选中的**异常，但要比我已经改变的方法的行为更方便(它将“不太等价”)。

# 更大的

> 但这远不是故事的全部。应用程序代码本身的系统异常怎么办？

当你写应用程序的时候，你应该什么时候抛出*系统异常*？

# 内部不变量

许多程序员使用注释来表明他们对程序行为的假设。例如:

在这种情况下，您可以使用 [Java 断言机制](/swlh/java-assertion-3b3c9611e1dc)。你可以像这样重写前面的 if 语句:

> *顺便注意，如果* `*i*` *为负，则上述示例中的断言可能会失败，因为* `*%*` *运算符不是真模运算符，而是计算余数，余数可能为负。*

[https://docs . Oracle . com/javase/8/docs/technotes/guides/language/assert . html](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html)

这个`assert`语句服务于运行时的验证(当断言通常通过使用-ea 标志到`java`来启用时)和意图文档。它是**可读的**并且它的性能影响很小([欧几里德算法](https://en.wikipedia.org/wiki/Euclidean_algorithm)非常快，不管你是应用它 2 次还是 3 次都没关系)。

> *尽管如此，* ***备选方案，是具有良好代码覆盖率的单元测试。你应该有一个涵盖 else 语句的单元测试。***

[https://medium.com/swlh/java-assertion-3b3c9611e1dc](/swlh/java-assertion-3b3c9611e1dc?source=your_stories_page-------------------------------------)

这是自引自关于 [Java 断言机制](/swlh/java-assertion-3b3c9611e1dc)的帖子。

# 内部不变量(开关)

> *另一个很好的断言候选是没有* `*default*` *格的* `*switch*` *语句。一个* `*default*` *案例的缺席通常表明程序员相信其中一个案例将总是被执行。假设一个特定的变量将有一个小数量的值是一个不变量，应该用一个断言来检查。例如，假设下面的* `*switch*` *语句出现在一个处理扑克牌的程序中:*

> *它可能表示一个假设，即* `*suit*` *变量只有四个值中的一个…*
> 
> *一个可接受的替代方案是:*

> *即使断言被禁用，这种替代方案也会提供保护，但是* ***额外的保护不会增加成本*** *:除非程序失败，否则* `*throw*` *语句不会执行。此外，在* `*assert*` *语句不合法的某些情况下，替代语句是合法的。如果封闭方法返回值，则* `*switch*` *语句中的每个 case 都包含一个* `*return*` *语句，并且* `*switch*` *语句后面没有* `*return*` *语句，那么添加带有断言的默认 case 会导致语法错误。(如果没有匹配的大小写并且断言被禁用，则该方法将返回无值)。*

[https://docs . Oracle . com/javase/8/docs/technotes/guides/language/assert . html](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html)

有趣的是，Oracle 并没有遵循该文档的建议，而是在它的指南示例中使用了`IllegalStateException`。例如参见 [Java 语言更新开关表达式](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html)。以下是更多相关信息。

引用自 [Java 语言更新开关表达式](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html):

> ***穷尽性***
> 
> *与* `*switch*` *语句不同，* `*switch*` *表达式的用例必须是穷举的，这意味着对于所有可能的值，必须有一个匹配的开关标签。由此可见，* `*switch*` *表达式通常需要一个* `*default*` *子句。但是，对于覆盖所有已知常数的* `*enum*` `*switch*` *表达式，编译器会插入一个隐式的* `*default*` *子句。*

[https://docs . Oracle . com/en/Java/javase/14/language/switch-expressions . html](https://docs.oracle.com/en/java/javase/14/language/switch-expressions.html)

引自 JEP 325:

> *例中的一个* `*switch*` *表达式必须详尽无遗；对于任何可能的值，都必须有一个匹配的开关标签。实际上，这通常意味着需要一个* `*default*` *子句；然而，在覆盖所有已知情况的* `*enum*` `*switch*` *表达式的情况下(最终，* `*switch*` *表达式覆盖密封类型)* **，编译器可以插入一个** `*default*`子句，指示`*enum*`定义在编译时和运行时之间已经改变。(这是当今开发人员手工做的事情，*但是让编译器插入它不仅干扰较少，而且可能比手工编写的错误消息* 更具描述性。)

[https://openjdk.java.net/jeps/325](https://openjdk.java.net/jeps/325)

所以，Oracle 在`enum` `switch`表达式中说“不要用`default`子句费心”，编译器会 a)隐式地添加它，b)它会抛出一些异常，c)它会有描述性的错误消息。

例如，科特林从一开始就没有“经典”`switch`的说法。相反，它使用`[when](https://kotlinlang.org/docs/reference/control-flow.html#when-expression)` [表达式](https://kotlinlang.org/docs/reference/control-flow.html#when-expression)，这将类似于 Java 的`*enum*` `*switch*` *表达式。*

很明显，Java 团队试图“修复”`switch`，特别是**完全摆脱抛出未经检查的异常的“负担”。**

实际建议如下:

*   如果你有`enum` `switch`表情，考虑在`switch` 表情发布时使用增强版。
*   抛出一些**未选中的**异常(可以是`**AssertionError**`，但不一定是**)与** `**default**` **子句。**

一些个人的注意，实际上这个用例是促使我使用 Java 断言机制的原因。

# 控制流不变量

假设您有一个如下所示的方法:

你可以把最后的注释替换成**抛出一些未检查的异常。**

可以是`*AssertionError*` 也可以是例如`IllegalStateException`。这并不重要，因为无论如何都不应该执行这个异常。

例如:

[https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/util/ReflectionUtils.java](https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/util/ReflectionUtils.java)

在这里，你可以看到`IllegalStateException`被抛出是为了取悦编译器。

**注意**:你不能让单元测试覆盖上面的第 20 行。

**注意:**在单元测试中，你不应该覆盖**所有**可能的执行分支。例如，考虑以下代码:

```
Method method = ...
Parameter[] parameters = method.getParameters();int length = (parameters==null)?0:parameters.length;String[] parameterNames = new String[length];Parameter param = null;for (int i = 0; i < parameters.length; i++) { param = parameters[i]; if (!param.isNamePresent()) { return null; } parameterNames[i] = param.getName();}
```

受[启发 https://github . com/spring-projects/spring-framework/blob/master/spring-core/src/main/Java/org/spring framework/core/standardfreflectionparameternamediscoverer . Java](https://github.com/spring-projects/spring-framework/blob/master/spring-core/src/main/java/org/springframework/core/StandardReflectionParameterNameDiscoverer.java)

**问题:**参数为空的情况是否应该编写单元测试？

**回答:**当然不是，如果你看了`getParameters()`方法的 java-doc 你会发现它的契约明确声明`null`不能返回(长度为 0 的数组可以)。所以，这个测试只是浪费时间。

你可以再问一个问题:*为什么代码里有一个* `*null*` *的检查？*如果合同说`null`不退？嗯，这是个好问题。有意见认为，这种检查应该避免。尤其是，当使用的 API 是 JDK 本身的时候。如果 API 合同规定`null`不会被退回，我们收到`null`的可能性极小。当然最后的说法是`true` 为高质量 API，但 JDK 绝对是高质量 API。取消这种检查的另一个原因是，当使用某个 API 时，需要进行大量的验证，99%的情况下都是浪费时间(无论是开发时间还是运行时间)。(当你验证一切并且不信任任何人时的编程风格被称为*防御性编程；*有时是好的，但一般情况下是不建议的)。

嗯，我最多同意上面提供的观点。但是我认为做一些简单检查来避免应用程序崩溃没有什么坏处。我有两个相反的论点:*错误*和*推理的简易性。*

**Bug 论证。**如果不管 API 契约说什么，方法总有一天会返回`null`呢？它甚至可以发生在 JDK，在一些 JDK 更新程序中可以引入 bug。同样，如果检查很简单，为什么不采取积极主动的措施并采取一些防御措施呢？倒楣的事情发生了…

**推理的容易程度。**大多数时候，你会调用一些质量不明确的 API。更重要的是，*你不想花时间去搞清楚是一个成熟的开源项目还是一个人维护的项目*。你可以争辩说，你应该只使用建立良好的高质量的代码库，但有时你没有一些选择。例如，你有一些利基要求，有人写了一个答案作为开源，但它不是很好地建立项目。或者你得到了这个 API 作为一个成熟项目的可传递依赖，它包含了对你直接有用的功能。所以，当你读代码时，你不想想，“嗯，在这种情况下，我使用 JDK，所以我可以省略`null`检查，在这种情况下，它是一些未知的 API，所以`null`检查应该把它的位置”。如果验证很简单，那么把它放在每一个地方就简单多了，当你*读*代码时(我们读代码的次数比写代码的次数多),就不用麻烦了。否则，可能是程序有问题，可能是你的代码调用了一些不可信的代码，而`null`检查被忘记了？

你可以在 [Java 断言机制](/swlh/java-assertion-3b3c9611e1dc)中找到更多的例子，但我个人从未在实践中使用过它们。

的扩展和更新版本

[https://www . toalexsmail . com/2009/07/handling-exceptions-in-J2EE-environment . html](https://www.toalexsmail.com/2009/07/handling-exceptions-in-j2ee-environment.html)

[https://www . toalexsmail . com/2009/07/handling-exceptions-in-J2EE-environment _ 24 . html](https://www.toalexsmail.com/2009/07/handling-exceptions-in-j2ee-environment_24.html)