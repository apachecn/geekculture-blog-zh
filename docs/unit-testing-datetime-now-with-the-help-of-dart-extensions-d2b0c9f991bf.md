# 借助 Dart 扩展对 DateTime.now()进行单元测试

> 原文：<https://medium.com/geekculture/unit-testing-datetime-now-with-the-help-of-dart-extensions-d2b0c9f991bf?source=collection_archive---------29----------------------->

![](img/c7d86c8e7524a2242fdceb13d68f7662.png)

# 问题是…

在我们的应用程序中，无论是在 UI 端还是在数据端，我们经常需要为日期设置“默认值”，在大多数情况下，最简单的方法是使用 DateTime.now()，毕竟，我们最有可能使用当前时间作为默认值。

虽然这非常方便，但是在测试的时候会造成复杂性。在 Dart 中，不可能简单地全局覆盖方法或函数，只能模拟传入类中的参数，向类中添加 DateTime 参数虽然很容易，但并不是很少需要，而且很可能会导致不必要的膨胀。

然而，对于小部件，情况就完全不同了，您要么需要将 DateTime 传递给小部件，要么创建一个自定义类，并将其添加到一些依赖注入中，以便能够在测试中模拟它。

# 一个解决方案…

我发现的一个解决方案是 dart 2.7 中引入的扩展

> Dart 2.7 中引入的扩展方法是向现有库添加功能的一种方式。( [*链接*](https://dart.dev/guides/language/extension-methods) *)*

使用它，我们能够在 DateTime 上创建一个扩展，用它我们既可以获得“当前”时间，又可以实现一个 setter 来允许我们覆盖“当前”时间。

```
extension ExtendedDateTime on DateTime { static DateTime? _customTime; static DateTime get current { return _customTime ?? DateTime.now(); } static set customTime(DateTime customTime) { _customTime = customTime; } }
```

我们有一个可空的 DateTime，当我们在代码中使用 ExtendedDateTime.current 时，它将返回 DateTime.now()的值。

这本身没什么特别的，set 方法才是我们真正关心的。

以下面这个类为例。

```
class User { final String userName; final DateTime createdDate; User(this.userName, this.createdDate); static User createUser(String userName) => User(userName, DateTime.now()); }
```

在我们代码的逻辑中，我们希望创建一个新用户，所以我希望验证当我调用 User.createUser 时，它会返回一个新用户，并带有传入的名称和当前日期时间。然而，在测试执行期间，调用函数和验证预期之间会有几毫秒的时间。

```
final result = User.createUser('Reme'); expect(result, equals(User('Reme', DateTime.now());
```

由于这几毫秒的时间，上面的例子几乎肯定会失败。

既然我们已经将扩展添加到项目中，我们可以重构代码，使其看起来更像:

```
class User { final String userName; final DateTime createdDate; User(this.userName, this.createdDate); static User createUser(String userName) => User(userName, ExtendedDateTime.current); }
```

之后，我们的测试看起来会像这样:

```
ExtendedDateTime.current = DateTime.parse( "2020-05-15 13:07:53.531Z", ); final result = User.createUser('Reme'); expect(result, equals(User('Reme', "2020-05-15 13:07:53.531Z");
```

# 最后的想法…

正如你所看到的，通过对我们的代码做很小的改动，我们现在可以很容易地保持事情的简单，而不会影响我们测试它们的能力。

我经常喜欢说，也许有人对我说过一次，不记得了…

> *工作代码！=可测试代码，但可测试代码==工作代码。*

这可能并不总是有趣的，但是今天几分钟的测试可以节省你明天几个小时的调试时间。

我希望你喜欢这篇文章，如果你有任何问题，意见或建议，请随时发表评论。

如果你喜欢，一颗心会很棒，如果你真的喜欢，一杯[咖啡](https://www.buymeacoffee.com/remelehane)会很棒。

感谢阅读。

*原载于 2021 年 7 月 18 日*[*https://remelehane . dev*](https://remelehane.dev/posts/unit-testing-dattimenow-with-the-help-of-dart-extensions/)*。*

[](https://itnext.io/flutter-web-should-i-use-it-part-1-seo-842d87ff9d28) [## Flutter Web:我应该使用它吗？(第 1 部分—搜索引擎优化)

### 有很多关于网页抖动和它的缺点的讨论，在这一部分我们将讨论 SEO 和网页清理器。

itnext.io](https://itnext.io/flutter-web-should-i-use-it-part-1-seo-842d87ff9d28) [](https://blog.usejournal.com/developing-on-an-m1-mac-flutter-563c8dcc28f) [## 在 M1 Mac 上开发(颤振)

### 作为一名开发新款 M1 Macbook Pro 的 Flutter and React 开发人员，我有一个简短的见解

blog.usejournal.com](https://blog.usejournal.com/developing-on-an-m1-mac-flutter-563c8dcc28f)