# 基于 React 维护干净的代码—第 2 部分

> 原文：<https://medium.com/geekculture/maintain-a-clean-code-based-in-react-part-2-b13323bfbd94?source=collection_archive---------34----------------------->

![](img/14c4252d929811dc16b0a6065f572039.png)

# 前言

我相信几乎每个开发人员都遇到过这样的情况，你从前一个开发人员那里接受了一个项目，但下一分钟你就陷入了困境，因为你不理解他们写的代码。

糟糕的命名约定，混乱的代码逻辑，冗余的代码，没有注释等等。

你会慢慢被这些乱七八糟的东西淹没，然后你会从头开始，从头再来。

在上一篇文章中，我们提到了一些关于如何提高和维护基于。现在，我们将继续深入这个话题。让我们看看如何继续利用我们代码的标准！

点击此处阅读:[第一部分](https://unicorn-sky.medium.com/maintain-a-clean-code-based-in-react-26681019448)

# 1.清晰的命名约定

![](img/3ce1573e4c30e1e91c0e928683683d79.png)

Photo by [Call Me Fred](https://unsplash.com/@callmefred?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/standard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你在一个团队中工作，在你的函数和变量中有一个清晰具体的命名约定是非常重要的，这样其他开发人员第一眼就能理解。

在创造名字的时候设定一个标准语言，比如英语，会很棒。遵循已设定的标准语言。尽量避免自己的母语(*非英语*)或母语，因为你可能会与来自不同国家的开发人员合作。除非你能 100000%保证你的项目只雇佣本地开发者。

如果可能的话，尽量避免使用缩写形式，因为不是每个人都能理解，包括几个月后当你回头看代码时的你自己。

除此之外，你可以考虑用复数来表示数组。假设你有一个电影列表，你可以将数组声明为`movies`而不是`movieList`。你把它命名为`movies`不是可读性更好吗？

而对于`boolean`值，也可以考虑 ***判断样式命名*** 。这意味着名字本身就像一个问题，你可以根据观察得到一个 ***是*** 或 ***否*** 的答案。例如，你有一封电子邮件，你想知道它的格式是有效还是无效。在这种情况下，可以考虑像`isValidEmail`一样声明函数或变量名。其他情况也是如此。如果您需要一个变量来检查图像的存在，您可以将它声明为`hasImage`。

我知道想一个好名字会消耗你的一些脑力。但是，尽量保持你所有的命名惯例 ***简短&***。

下面是一些命名约定的一个不太好的例子:

```
// Functionsconst getTZ = () => {};  // What is 'TZ'?
const getLocationList = () = return 'array'; // Too long
const getInputValidation = input => return 'boolean'; // Less specific// Variablesconst x = '100ml';      // What is the usage of 'x'?
const zaoCanDian = '';  // What is 'zaoCanDian'? 
const cinemaLocationList = [];  // Can be shorten
```

这是我们改进它们的方法:

```
// Functionsconst getTimezone = () => {};
const getLocations = () = return 'array'; 
const isInputValidated = input => return 'boolean';// Variablesconst waterVolume = '100ml';
const breakfastShop = '';
const cinemaLocations = [];
```

总之，在创建名称时，您应该注意以下几点:

*   设定标准语言。
*   避免简短形式。
*   使用复数表示数组。
*   使用判断式命名。
*   避免使用不符合语言标准的语言。

> 清晰、具体、简短而简单

# 2.利用条件语句

![](img/2270736fc914da1f31316ded87d795b7.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/yes-or-no?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

与其使用传统的`if...else`语句，为什么不考虑使用`? || &`等条件运算符呢？它将把你从嵌套的`if...else`地狱循环中拯救出来。

此外，当您处理条件组件时，这也有助于减少`useEffect`的使用。例如，您想在成功的 API 请求时显示一个组件，而不是用`useState`切换组件，并使用`useEffect`控制 DOM 更新，您可以尝试如下方法:

```
{ **isSuccess** && <div> Only display if successful </div> }{ **isSuccess** ? <div> Component A </div> : <div> Component B </div> }
```

下面的格式解释了条件是如何工作的:

> const**is success**=[条件] === 'TRUE '？返回真:返回假；

您可以在许多地方使用这种做法，包括您的函数、参数检查等等。现在，让我们快速看一下如何创建一个检查偶数的 JS 函数。如果数字是偶数，函数应该返回 true。

这里有一个不太好的例子:

```
const **isEven** = *number* => {
  let result = '';if(number % 2) {
    result = true;
  } else {
    result = false;
  }return result;
}
```

增强版本:

```
const **isEven** = *number* => number % 2 === 0;
```

一句台词就行。很神奇，不是吗？

但是，有一点需要注意。如果使用条件操作符返回*类名*，如果条件无效，确保返回一个空字符串。否则，您将在 classname 中得到一个 ***false*** 值，您可以在检查元素时看到它。

```
// Not-so-good
const className = [condition] && 'bg-blue';<div class="***false*** container">Your component</div>// Better
const className = [condition] ? 'bg-blue' ? '';<div class="container">Your component</div>
```

关于条件操作符的用法，还有很多可以探索的地方。如果你想了解更多，就去谷歌一下吧！

# 3.神奇的数字宣布

![](img/4fe61c136e3de5016e69453cf433c703.png)

Photo by [Sigmund](https://unsplash.com/@sigmund?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/numbers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

嗯，什么是“ ***幻数*** ”？**幻数**是代码中出现的任意随机数，没有任何具体声明。它没有明确的含义。看看下面的代码就知道了:

```
if (responseCode === 00) {
  console.log('test');
}
```

你能告诉我这里发生了什么吗？什么是“00”？因此，为了更好的可维护性和可读性，最好声明这个数字。例如。

```
const FAILED_RESPONSE = 00;if (responseCode === FAILED_RESPONSE) {
  console.log('test');
}
```

现在看起来清晰易懂多了。试着实践这种行为，它真的对你的发展有很大帮助。

# 4.分离常量和虚拟数据

![](img/381f2f25f805613b2095d6ec3e10bea4.png)

Photo by [Jason Leung](https://unsplash.com/@ninjason) on Unsplash

我称它们为**CD**，但这并不是我们过去所知道的*光盘*或*克里斯汀迪奥*的典型含义。事实上，它们是:

> 常量变量和虚拟数据

全局变量是项目中经常使用的某些变量。通常，我会将全局常量变量拆分到一个单独的文件中，然后将其放在`constants`文件夹下。`constants/global.js`如。

这同样适用于虚拟数据，例如为测试目的而创建的*数组*、对象的*数组和*对象*。首先，我会在`constants`文件夹下创建一个临时的模拟文件，例如`mock.js`，然后将数据分离并保存在这里。*

这是另一个保持代码整洁的实践，因为我只需要在我想使用它的时候导入它。

# 结论

![](img/08de6ba567b1c0cbc2f231d16ae1fd23.png)

Photo by [Fab Lentz](https://unsplash.com/@fossy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/clear-name?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

每一个改变都需要时间。一开始可能会有点匆忙，但开始永远不会晚。

感谢阅读！如果你喜欢我的文章，请给我一些掌声！

如果你想把我的文章翻译成另一种语言，欢迎你来做！请在你的帖子中表扬我，并在评论框中给我留言！

祝你旅途顺利！祝你愉快，干杯！