# 马上使用这 8 个新的 JavaScript 特性。

> 原文：<https://medium.com/geekculture/use-these-8-new-javascript-features-right-away-1cdfa30a644a?source=collection_archive---------7----------------------->

## 顶级 await、RegExp 匹配索引、新的公共和私有类字段以及其他创新都是 ECMAScript 2022 中的新特性。

6 月 22 日，ECMAScript 2022 (ES13)发布，编纂了最新一轮的新 JavaScript 功能。每一个技术定义都标志着实际应用的一个转折点。随着 JavaScript 被开发人员使用，我们不断发现新的可能性并发展这种语言。作为回应，ECMAScript 标准将附加功能形式化。这些反过来又为 JavaScript 的持续发展创造了一个新的起点。

对于 JavaScript，ES13 标准增加了八项新功能。

让我们从您可以立即利用的新功能开始。

# 类别字段

类字段的概念统一了管理 JavaScript 类成员的各种增强，包括静态类特性、私有实例方法和访问器，以及类公共和私有实例字段。

# 公共和私有实例字段

以前，在关键字`class`中定义成员字段通常包括将它添加到构造函数中。ECMAScript 的最新版本允许我们在类体内声明成员字段。我们可以使用 hashtag 来指定私有字段，如清单 1 所示。

## 清单 1。内联公共和私有类字段

```
**class** **Song** {
    title = "";
    *#artist = "";*
    constructor(title, artist){
      **this**.title = title;
      **this**.#artist = artist;
    }
}
**let** song1 = **new** **Song**("Only a Song", "Van Morrison");
console.log(song1.title);
*// outputs “Only a Song”*
console.log(song1.artist);
*// outputs undefined*
```

使用 class 关键字，我们在清单 1 中定义了类 Song。标题和艺术家是这个班仅有的两个人。艺术家成员前面的井号(#)表示它是私有的。这些字段可以在我们支持的构造函数中设置。请记住，构造函数必须能够访问这个。#artist，以防止该字段被替换为公共成员。下一步是定义一个 Song 类实例，在构造函数中设置这两个字段。然后这些字段被打印到控制台。关键是 song1.artist 输出是未定义的，对外界是不可见的。还要记住，song1.hasOwnProperty("artist ")将返回 false。此外，利用赋值不允许我们随后在类上构造私有字段。

这是一个非常好的特性，它可以使代码整体更加整洁。大多数浏览器早就支持公共和私有实例字段，所以很高兴看到它们被正式采用。

# 私有实例方法和访问器

此外，方法和访问器可以以散列符号为前缀。与私有实例字段类似，对可见性的影响是相同的。因此，如清单 2 所示，您可以向`Song.artist`属性添加一个私有 setter 和一个公共 getter。

## 清单 2。私有实例方法和访问器

```
**class** **Song** {
  title = "";
  *#artist = "";*
  constructor(title, artist){
    **this**.title = title;
    **this**.#artist = artist;
  }
  **get** getArtist() {
    **return** **this**.#artist;
  }
  **set** *#setArtist(artist) {*
    **this**.#artist = artist;
  }
}
```

# 静态成员

类字段建议还引入了静态成员。它们的外观和工作方式类似于它们在 Java 中的工作方式:如果一个成员有`static`关键字修饰符，它就存在于类中，而不是对象实例中。您可以向`Song`类添加一个静态成员，如清单 3 所示。

## 清单 3。向类中添加静态成员

```
**class** **Song** {
  **static** label = "Exile";
}
```

该字段只能通过类名`Song.label`访问。与 Java 不同，JavaScript 实例不包含对共享静态变量的引用。注意，有可能有一个带有 static `#label`的静态私有字段；也就是私有静态字段。

# 正则表达式匹配索引

正则表达式`match`已经[升级](https://github.com/tc39/proposal-regexp-match-indices)以包含更多关于匹配组的信息。出于性能原因，只有在正则表达式中添加了`/d`标志时，才会包含这些信息。(请参见 ECMAScript 提案的 [RegExp 匹配索引，深入了解`/d` regex 的含义。)](https://github.com/tc39/proposal-regexp-match-indices)

基本上，使用`/d`标志会导致 regex 引擎包含所有匹配子字符串的开始和结束。当标志存在时，`exec`结果上的`indices`属性将包含一个二维数组，其中第一维表示匹配，第二维表示匹配的开始和结束。

在命名组的情况下，索引将有一个名为`groups`的成员，其第一维包含组的名称。考虑清单 4，它取自提案中的[。](https://github.com/tc39/proposal-regexp-match-indices#examples)

## 清单 4。正则表达式组索引

```
**const** re1 = /a+(?<Z>z)?/d;*// block 1*
**const** s1 = "xaaaz";
**const** m1 = re1.**exec**(s1);
m1.indices[0][0] === 1;
m1.indices[0][1] === 5;
s1.slice(...m1.indices[0]) === "aaaz";*// block 2*
m1.indices[1][0] === 4;
m1.indices[1][1] === 5;
s1.slice(...m1.indices[1]) === "z";*// block 3*
m1.indices.groups["Z"][0] === 4;
m1.indices.groups["Z"][1] === 5;
s1.slice(...m1.indices.groups["Z"]) === "z";*// block 4*
**const** m2 = re1.**exec**("xaaay");
m2.indices[1] === **undefined**;
m2.indices.groups["Z"] === **undefined**;
```

在清单 4 中，我们创建了一个匹配`a`字符一次或多次的正则表达式，后面是一个匹配`z`字符零次或多次的命名匹配组(名为`Z`)。

代码块 1 演示了`m1.indices[0][0]`和`m1.indices[0][1]`分别包含 1 和 5。这是因为正则表达式的第一个匹配是从第一个`a`到以`z`结尾的字符串。第 2 块显示了`z`角色的相同情况。

块 3 显示了通过`m1.indices.groups`访问带有命名组的第一维。有一个匹配的组，即最后的`z`字符，它的开头是 4，结尾是 5。

最后，块 4 演示了不匹配的索引和组将返回 undefined。

底线是，如果您需要访问字符串中匹配组的细节，您现在可以使用 regex 匹配索引特性来获得它。

# 顶级等待

ECMAScript 标准现在允许打包异步模块。当您导入一个包含 await 语句的模块时，包含的模块不会运行，直到所有的等待都被满足。当处理相互依赖的异步模块调用时，这可以防止可能的竞争问题。有关更多信息，请参见顶级 await 建议。

## 清单 5。顶级等待

```
*// awaiting.mjs*
**import** { process } **from** "./some-module.mjs";
**const** **dynamic** = **import**(computedModuleSpecifier);
**const** data = fetch(url);
**export** **const** output = process((await **dynamic**).**default**, await data);
*// usage.mjs*
**import** { output } **from** "./awaiting.mjs";
**export** **function** outputPlusValue(value) { **return** output + value }console.log(outputPlusValue(100));
setTimeout(() => console.log(outputPlusValue(100), 1000);
```

注意在`awaiting.mjs`中`await`关键字前面使用了依赖模块`dynamic`和`data`。这意味着当`usage.mjs`导入`awaiting.mjs`时，`usage.mjs`不会被执行，直到`awaiting.mjs`依赖项完成加载。

# 针对私人领域的人体工程学品牌检查

作为开发人员，我们想要舒适的代码——因此[符合人体工程学的私有字段](https://github.com/tc39/proposal-private-fields-in-in)。这个新特性让我们可以检查一个类中是否存在私有字段，而无需求助于异常处理。

清单 6 展示了这种新的、符合人体工程学的方法，使用`in`关键字从类中检查私有字段。

## 清单 6。检查私有字段是否存在

```
**class** **Song** { 
  *#artist;* 
  checkField(){ 
    **return** *#artist in this;*
  } 
}
**let** foo = **new** **Song**();
foo.checkField(); *// true*
```

清单 6 是人为的，但是想法很清楚。当您发现自己需要检查一个类的私有字段时，可以使用格式:`#fieldName in object`。

# 负索引。在()

`arr[arr.length -2]`的日子一去不复返了。[。内置索引的 at 方法](https://github.com/tc39/proposal-relative-indexing-method)现在支持负索引，如清单 7 所示。

## 清单 7。带的负索引。在()

```
**let** foo = [1,2,3,4,5];
foo.at(3); *// == 3*
hasOwn
```

# 哈苏恩

Object.hasOwn 是 Object 的更好的迭代。hasOwnProperty。支持某些边缘环境，例如使用 Object.create 创建对象时(空)。请记住，静态函数 hasOwn 不存在于实例中。

## 清单 8。hasOwn()正在运行

```
**let** foo = **Object**.create(**null**);
foo.hasOwnProperty = **function**(){};
**Object**.hasOwnProperty(foo, 'hasOwnProperty'); *// Error: Cannot convert object to primitive value*
**Object**.hasOwn(foo, 'hasOwnProperty'); *// true*
```

清单 8 显示了您可以在用`Object.create(null)`创建的`foo`实例上使用`Object.hasOwn`。

# 错误原因

更不用说，原因支持现在是错误类的一部分。这使得错误链具有类似于 Java 的堆栈跟踪。如清单 10 所示，错误构造函数现在接受带有原因字段的选项对象。

## 清单 10。错误原因

```
**throw** **new** **Error**('Error message', { cause: errorCause });
```