# 从编程开始！

> 原文：<https://medium.com/geekculture/beginning-with-programming-34bfc0110626?source=collection_archive---------1----------------------->

编程是一门艺术，不是一年左右就能学会的。你需要不断地、有规律地练习才能熟练。但是，在成为职业选手之前，一些基本的基础是很重要的。本指南将告诉你一些编程时可以使用的有效实践。

听起来很刺激？让我们开始吧！

# **1。什么是编程？**

我不会在这里花太多时间，因为如果你正在阅读这篇文章，你可能对什么是编程和编码有一个相当坚实的概念。

它只是逻辑的形式化版本，在机器可以解释的语言的词汇和语法中。“我想看看这个数据库表中的所有行”变成了`SELECT * FROM table`。是技术的实现层面。所以，为了更好地编程，你将会写很多代码！

# 2.为什么要学编程？

如果你只是通过阅读这一部分的标题就找到了答案，这是一个很好的迹象。人们不坚持学习任何技能的最大因素之一是因为他们的“为什么”要么不存在，要么不明确。

*知道你的为什么*。学习编程的主要想法是:找到一份更好的工作，快乐，有影响力，经济上有保障，发现新的爱好或激情，等等。无论你想到什么，都要深入细节。记住，你的答案越清晰，你学习编程或其他技能的动力就越大。

# 3.如何练习编程？

有些技能可以有更高的知识 v/s 应用比率。这是**而不是**编程的情况。编程是一项技能，你需要更多的实践，而不是仅仅停留在理论上。

我建议你至少花 80%的时间写代码，20%的时间学习理论。随着你学的越来越多，我建议增加练习的比例。简而言之，**你花在试图理解概念上的每一分钟，你都应该多花 5 倍的时间将其付诸实践**。这就是这些概念如何深入你的大脑并开始变得有意义；而不是通过反复阅读。

记住:**不仅仅是打出代码。**这也是关于修复 bug、维护、安全、测试、用户体验、架构、系统、数据库和其他各种各样的事情。事情变得复杂了。**学习编码只是冰山一角。**

首先，编程不是一件容易的事情，尤其是如果你过去没有进行过很多其他逻辑技能的练习。这就是为什么擅长数学通常是进入计算机编程程序的一个要求。您在编程中使用的数学很少那么复杂，但是您通过应用数学学到的逻辑大大提高了您的编程能力。

# 4.如何打破会话？

这很大程度上取决于你的情况。你们中的一些人经过一个月的练习就能做到足够的熟练，而另一些人根本花不了多少时间。对于大多数技能，我建议每天至少练习 15 分钟。但是对于编程来说，这还不够。对于编程，如果你每天练习 30 分钟，那就是每周 3.5 小时或每月 14 小时。

虽然一切都历历在目，事情看起来很容易，但如果你停止练习，哪怕只是一个月，你会失去大约 60%你以前学过的东西。黄金法则是*间隔学习*和*重复*。你应该足够经常地回忆你所学的东西。

# 5.什么时候练习编程？

就一个字的回答——“每天**”**

**你越能让你的技能练习成为一种习惯，就越容易坚持下去并取得成效。例如，专注 30 分钟总是比专注于写一段复杂的代码更有动力。但是既然编程是一门复杂的技能，一定要在思维最敏捷的时候练习。**

**虽然大多数程序员自称是夜猫子，但研究表明，大多数人在醒来后不久思维最敏捷。如果你不确定什么时候适合你，我会从清晨开始。如果不能马上奏效，请不要放弃，换个时间试试。新习惯的形成需要时间。**

> **使用不同的语言、框架和 ide 进行练习。试试结对编程。做函数式编程，OOP，组件式编程。试试脚本或者 ML。在你的房间、厨房、咖啡馆、学校、室外等地方编码。尝试不同类型的音乐。你能做的事情有无限的组合！**

# **6.编程的基本概念**

**说实话，编程的技巧太宽泛了。如果你想学“编程”，你会很快失去动力。把它看成一个单一的实体，简直超乎想象！因此，我们将把它分成以下子技能:**

## ****6.1 布尔和条件逻辑****

**布尔表达式的值是二进制值`true`或`false`。在大多数语言中，这些都是用等号来表示的。假设，`var a = 3`那么`print(var)`会给出 3 作为输出。但是`print(var == 3)`会打印出`true.`这是因为`var == 3`是一个布尔条件，将 var 的值与 3 进行比较，然后返回输出。**

***条件逻辑*是布尔表达式的求值。基本上，`if`这不是做`then`那。一个条件表达式在大多数语言中有多个由`and/or.`定义的部分，“与”由`&&`定义，“或”由`||`定义。**

**还可以用`(`和`)`对布尔表达式进行分组。`if (a || (b && c)) then d`。`if (kick the imposter || (task completed && crew members are alive)) then crew will win.`**

**正如你可能想象的那样，这可能会变得非常复杂。在本文中，我将向您展示优秀的程序员如何让阅读他们代码的人容易理解这一点。**

## **6.2 变量**

**简单来说，变量就是保存一个值的东西。示例:**

```
int numImposter = 1;
string imposterColor = "Black";
bool isImposter = true;
string[] crewColor = {"Cyan", "Brown", "Black", "Green", "Blue"};
float deadCrewmates = 4.0;
int[] kickVotes = {2, 1, 3, 0, 1};
```

**优秀的程序员会以描述性的方式使用变量，而不是“硬编码”值。下面是一个“硬编码”值的示例:**

```
if (imposterKicked < 1) {
  // Continue the game
}
```

**对于不熟悉运行游戏或代码的人来说，`1`在这里意味着什么？没什么。现在，如果我们用变量来代替，就清楚多了:**

```
if (imposterKicked < numImposter) {
  // Continue the game
}
```

**我们同样可以用`crewColor`数组编写代码:**

```
if (deadCrewmates < (crewColor.length - numImposter + 1)) {
  // Continue the game
}
```

## **6.3 循环**

**以下数之和是多少:`2, 6, and 1`？相当简单，对吧？你是怎么做到的？大概是这样的:**

**2 + 6 = 8
8 + 1 = 9**

**在你的大脑中，这需要两次迭代。在这个非常简单的例子中，我们知道我们有多少个数字以及它们的值是多少。在编程中很少出现这种情况。**

**正如上一节简要提到的，您有了一个`array`的概念。你不知道有多少个值，值是什么。假设你想写一个算法(方法),在任何情况下都可以计算出某个人被踢出局的投票总数？答案是用一个叫做`loops`的概念。从根本上来说，这和你在上面的头脑中所做的没有任何不同。下面是代码的样子:**

```
int[] kickVotes = {2, 1, 3, 0, 1};
var totalVotes = 0;
for (var i = 0; i < kickVotes.length; i++) {
  totalVotes = totalVotes + kickVotes[i];
}
print totalVotes;
```

**最后一行会在你的屏幕上显示`7`。**

**上面的代码中发生了很多事情。一个`for`循环从一个索引到另一个索引逐个遍历数组的每个值。**

**在上面的例子中，我们从索引`0`开始(参见`var i = 0;`)。在大多数编程语言中，数组的第一个索引不是`1`，而是`0`。在上面的例子中，我们的索引范围是从`0`到数组中出现的值的数量，用`kickVotes.length`表示。本质上，`var i = 0; i < kickVotes.length; i++`意味着算法将逐个遍历每个索引(`i++`)，从第一个索引开始，直到最后一个索引。**

**所以，一个`for`循环有三个组成部分:起始索引；每次迭代的结束索引和步数，即`for (startIndex; endIndex; numSteps)`**

**`kickVotes[i]`表示循环当前所在索引处的值。如果你用`kickVotes[3]`代替这里的`i`，你将得到的值是`0`。上面的`for`循环基本上是这样执行的:`var totalVotes = kickVotes[0] + kickVotes[1] + kickVotes[2] + kickVotes[3] + kickVotes[4]`**

**`for`是最常见的循环。您还会经常发现`while`循环。意思是“做某事直到我做完”。示例:计算投票数，直到投票结束。假设我们想打印游戏中每个伙伴的投票结果:**

```
int memberIndex = 0;
while (memberIndex < kickVotes.length) {
  print kickVotes[memberIndex];
  memberIndex++;
}
```

**你认为这里会发生什么？你会在屏幕上看到:**

```
2
1
3
0
1
```

**您将看到所有 5 个值都被打印出来。`while`循环中的代码将一直执行，直到满足条件:`memberIndex < kickVotes.length`，即—到达数组末尾！**

## **6.4 功能**

**函数使代码片段可重用。还记得上面计算投票总数的`for`循环吗？你如何让它适用于任何游戏？让我们做一些调整:**

```
function countVotes(kickVotes) {
  var totalVotes = 0;
  for (var i = 0; i < kickVotes.length; i++) {
    totalVotes = totalVotes + kickVotes[i];
  }
  return totalVotes;
}
```

**您已经创建了您的第一个函数/方法！这本身实际上没有任何作用。下面是该函数的使用方法:**

```
int[] kickVotes1 = {2, 1, 3, 0, 1};
int[] kickVotes2 = {0, 3, 1, 5, 0, 0};
print countVotes(kickVotes1);
print countVotes(kickVotes2);
```

**这将在您的屏幕上打印 7 和 9**

# **7.良好的编程实践**

**上面的基础知识可以帮助你开始，但是你不会成为一个很好的程序员。下面，我一会儿就来解释一些从 noob 到 good 的方法。这一部分是最复杂的一部分，所以如果你目前不能跟上，不要忘了做书签，以便在你思维最敏捷的时候重温它！**

## **7.1 干净的代码**

**假设你正在 [Unity](https://unity.com/) 开发一款 2D 平台游戏。你想增加简单的范围探测来决定敌人何时攻击玩家。一个普通的程序员可能会写出这样的代码(如果不使用碰撞器的话):**

```
void Update() {
  if (Mathf.Abs(transform.position.x * 100.0 - player.transform.position.x * 100.0) <= 10.0) {
    isAttacking = true;
    animation.SetAnimation(0, "attack", false);
  }
}
```

**这让你害怕了吗？别担心，应该的。**写干净的代码是为了让它变得如此容易理解，以至于你只是在读英语**。上面的例子非常简单，但是我们可以做得更好。这里是一个更高级的程序员可能写它的一种方法:**

```
void Update() {
  if (IsCloseToPlayer())
     Attack();
}

bool IsCloseToPlayer() {
  return (PositionDifference(transform, player.transform) <= ATTACK_DETECTION_RANGE);
}float PositionDifference(Transform t1, Transform t2) {
   return Mathf.Abs(GetHorizontalPosition(t1) -  GetHorizontalPosition(t2));
}float GetHorizontalPosition(Transform t) {
   return t.position.x * PIXELS_PER_UNIT;
}void Attack() {
  if (isAttacking)
    return; 
  isAttacking = true;
  int animationTrack = 0;
  bool loop = false;
  animation.SetAnimation(animationTrack, "attack", loop); 
}const float PIXELS_PER_UNIT = 100.0; 
const float ATTACK_DETECTION_RANGE = 10.0;
```

**你注意到的第一件事是什么？“还有很多代码！!"，对吧？我花了更多的时间来写吗？你打赌！但这就是为什么它比第一块代码更好。**

## **7.2 抽象和封装**

*****隐藏实现细节*** :当其他人(或未来的你)阅读你的代码时，他们想在跳入细节之前先明白是怎么回事。有时候，细节并不那么重要。**

**如果您再次查看第二个代码块的 Update()函数:**

```
void Update() {
  if (IsCloseToPlayer())
    Attack();
}
```

**读起来很简单吧？尤其是在你之前看到的第一个版本旁边。如果你读了前面的代码块，你会花更多的时间去理解它的意思:“如果我离玩家很近，我应该开始进攻”。创建名副其实的小方法使你的代码可读性更好。**

## **7.3 命名每个片段**

**编写干净代码的经验法则是命名。程序员花 90%的时间为代码想一个合适的名字，可以是任何布尔条件、函数名等等。**

**例如:`IsCloseToPlayer()`比`Mathf.Abs(transform.position.x * 100.0 — player.transform.position.x * 100.0) <= 10.0`更容易阅读**

## **7.4 停止使用注释**

**大多数好的代码不需要注释来解释逻辑，因为变量和函数都被恰当地命名了。如果您需要一个注释来解释一段代码，那么您可以创建一个命名良好的方法来代替。所以，在你写评论之前要好好想想。**

## **7.5 功能分解**

**在真正尝试一个问题之前，试着把它分解成几个小问题，并为每个问题创建一个函数。这将增强代码的可读性和清晰性。**

## **7.6 7 的规则**

**一个函数/方法应该有多少行代码？简单回答一下:7 2。原因是我们的大脑只能同时处理这么多的指令。每当我看到超过 9 行代码的方法时，我都会退缩。**

## **7.7 概括**

**尽量不要硬编码这些值。代码越通用，就越具有可重用性。优秀的程序员通过将代码一般化来提高代码的可读性。**

**例如，在上面用于找出投票总数的代码中，我们使用了 length()方法来找出总数。相反，如果我们使用了一个常量值，比如 5，那么它就不能被其他不同大小的数组重用。**

# **8.卡住了**

**被困在某个地方本身就是一种技能，因为不犯任何错误，你就不会学到它。随着你不断学习，你会经常遇到[stackoverflow.com](https://stackoverflow.com/)或类似的网站。当你在谷歌上搜索如何在编程中做一些事情时，很可能其他人也有和你一样的问题，并且其他人给出了明确的答案以及如何修复它。**

**不知道每个答案并不丢人。编程的内容太多了，不可能面面俱到。在编程世界里，你总是被期望足智多谋，首先自己找出答案——这是学习任何东西的好方法。如果你试了之后还是找不到答案，那么是时候向别人寻求帮助了，或者亲自去，或者在 StackOverflow 上询问。**

# **9.最终提示**

***比较不同的代码。*试着理解哪一个在复杂性、可读性和通用性方面更有效率。*没有什么是最终的*。请始终尝试编写比早期版本更高效的代码。*不要只是学习编码，要学习如何成为一名开发者*。学会解决问题。**

****放松。又不是赛跑，不用急。如果你才刚刚开始，你不会因为不了解事情而“落后”任何人。当你刚学完字母表时，你不会写小说。所以，**尽情享受吧。******

**玩得开心！**