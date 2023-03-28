# Swift 5.5 中的时间控制

> 原文：<https://medium.com/geekculture/time-steering-in-swift-5-5-ae39fd9ccd09?source=collection_archive---------5----------------------->

![](img/a5b7f2cee4429148f220189486deb60b.png)

Photo by [Djim Loic](https://unsplash.com/@loic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在应用程序开发中有效地处理日期和时间是非常重要的。像 Twitter、WhatsApp 或 Instagram 这样的流行应用程序使用日期和时间格式以及许多其他操作功能来有效地传递信息。

从基本的操作功能到非常需要的格式化功能，我们将回忆一切。

## 向今天添加 n 个时刻

```
let tomorrow = Calendar.current.date(byAdding: DateComponents(day: 1), to: Date())let yesterday = Calendar.current.date(byAdding: .day, value: -1, to: Date())
```

通过更改第一个 Calendar.component 参数，我们可以前进/后退任意天数、月数、分钟数、小时数、秒钟数和年数。

## 下一个日期要求

当用户想知道下一个可能的日期是哪个月的第二天是星期二时。

```
Calendar.current.nextDate(after: Date(), matching: DateComponents(day: 2, weekday: 3), matchingPolicy: .strict, repeatedTimePolicy: .first, direction: .forward)
```

这样，向前和向后计算都是可能的。任何这样的日期、月份、年份、分钟或秒钟的排列要求都可以通过这个功能实现。

此外，上述方法中所有这些可能性的列举可以如下进行:

```
var i = 8// enumerate above resultCalendar.current.enumerateDates(startingAfter: Date(), matching: DateComponents(day: 2, weekday: 3), matchingPolicy: .strict, repeatedTimePolicy: .first, direction: .forward) { result, exactMatch, stop in print(result!) i = i-1 if i == 0 { stop = true }}
```

这给出了满足要求的下 8 个结果。

## 验证用户输入的日期

```
Calendar.current.isDateInTomorrow(tomorrow)Calendar.current.isDateInToday(Date())Calendar.current.isDateInWeekend(Date())Calendar.current.isDateInYesterday(yesterday)
```

使用上述方法和其他类似的方法，我们可以验证用户输入的日期是否在周末/明天/昨天。

## 按粒度比较

swift 中的两个日期时间对象可以在 swift 中使用比较运算符进行比较，例如或==。

```
Date() == Date() //trueDate() < tomorrow! //trueDate() < yesterday! //false
```

相反，如果你想仅通过任何组件进行比较，那么我们可以使用下面的

```
Calendar.current.compare(Date(), to: yesterday, toGranularity: .hour) == .orderedAscending
```

此外，如果我们想比较所有的时间组成部分，而不是只有一个，那么下面是一种方法

```
let date1  = Date()let components = Calendar.current.dateComponents([.hour, .minute, .second], from: tomorrow!)let date2 = Calendar.current.date(bySettingHour: components.hour!, minute: components.minute!, second: components.second!, of: Date())!
```

转换后的 datetime 对象 date2 和 date1 可以通过比较操作进行比较，或者通过下面的方法获得差异。

```
let difference = Calendar.current.dateComponents([.hour, .minute], from: date1, to: date2)
```

## 标志

另一个有用的方法是各种样式的月份和工作日的符号，可以用本地方法制作。

```
Calendar.current.weekdaySymbolsCalendar.current.shortWeekdaySymbolsCalendar.current.standaloneWeekdaySymbolsCalendar.current.veryShortWeekdaySymbols
```

## 格式化日期和时间

从 swift 5.5 开始，使用 DateFormatter 格式化的拖动方式被压缩方式所取代。

```
Date().formatted(date: .abbreviated, time: .shortened)
```

日期和时间样式的不同组合可以由简单的上述功能组成，包括省略任何组件。

特别是，如果您想要包含、排除或样式化单个组件，如包含日期和排除月份，可以使用以下内容

```
Date().formatted(.dateTime.day().weekday())Date().formatted(.dateTime.hour().minute().weekday(.wide))
```

**格式化间隔**

间隔的格式如下

```
(Date()..<tomorrow!).formatted(date: .numeric, time: .shortened) //"11/7/2021, 1:33 AM – 11/8/2021, 1:33 AM"(Date()..<tomorrow!).formatted(.timeDuration) //"23:59:59"(Date()..<tomorrow!).formatted(.components(style: .abbreviated)) //"23 hr, 59 min, 59 sec"(yesterday!..<Date()).formatted(.components(style: .condensedAbbreviated, fields: [.day])) //"1 day"
```

与上面的方法类似，使用 RelativeDateFormatter 的痛苦格式化也变得简单了，如下所示

```
tomorrow!.formatted(.relative(presentation: .named, unitsStyle: .wide)) //"tomorrow"tomorrow!.formatted(.relative(presentation: .numeric, unitsStyle: .wide)) //"in 1 day"tomorrow!.formatted(.relative(presentation: .numeric, unitsStyle: .abbreviated)) //"in 24 hr."
```

就是这样！

感谢阅读！！！

👉你觉得这个帖子有用吗？你现在可以通过[给我买一个披萨](https://www.buymeacoffee.com/maheshsai)来支持我坚持下去的努力！