# Python 初学者系列| Day-08

> 原文：<https://medium.com/geekculture/python-for-beginner-series-day-08-ecefaa800e9d?source=collection_archive---------11----------------------->

这里我们将进行编码活动

![](img/7f7607259f125729c06a42246cc3d3a9.png)

*   第八天，我们要做一个编码挑战“爱情计量器”。
*   这个编码活动主要涵盖了我们第 1 天到第 7 天的主题
*   让我们为爱情计画写代码

**说明:**

1.  获取人名作为两个输入
2.  组合这两个字符串输出结果
3.  一旦你得到两个人的名字，并检查单词**中的字母出现的次数，真的**。另外，检查单词 **LOVE** 中的字母出现的次数。
4.  如果爱情分数**小于**10*或*大于**90**，则显示分数和类似于`**you are made for each other like Hot chocolate and honey**"`的信息

5.如果分数在 40-50 之间，则显示类似于“`**you are alright together**`”的信息

6.否则只显示类似`**you are like Russia and ukraine"**`的信息

*   让我们写一个代码

```
print("Welcome to the Love Meter Coding Challenge!")name1 = input("What is your name? \n")name2 = input("What is their name? \n")
```

*   在上面的例子中，我们将通过**“输入函数”**获取人名作为输入，并分别为**name 1**&**name 2**赋值

```
combined_strings = name1 + name2
```

*   在这一步中，我使用“ **+** ”符号连接了两个字符串，我们已经知道当我们使用加号时，即使是一个数值，字符串也会连接。

```
string_lower_con = combined_strings.lower();
```

*   这里我们将把字符串值转换成小写，因为有时用户可能输入大写或大写，所以我们默认转换成小写

```
t = string_lower_con.count("t")
r = string_lower_con.count("r")
u = string_lower_con.count("u")
e = string_lower_con.count("e")true = t + r + u + e
```

*   让我们找出出现在单词“ **true** 中的人名字母数。为此，我们将使用 count 函数和上述字母，现在我们得到了单词 true 的第一个分数。

```
l = string_lower_con.count("l")
o = string_lower_con.count("o")
v = string_lower_con.count("v")
e = string_lower_con.count("e")love = l + o + v + e
```

*   用“**爱**也重复同样的事情

```
love_score  = int(str(true) + str(love))
```

*   到目前为止，我们可以得到分数，但仍然需要显示基于“ **love_score** ”的消息。

```
if (love_score < 10) or  (love_score > 90):
     print(f"your love meter score is {love_score},**you are made for each other like Hot chocolate and honey")**elif (love_score >=40) and (love_score<=50)
     print(f"your love meter score is {love_score},**you are alright together)**else:
    print(f"your love meter score is {love_score},**you are like russia and ukarine)**
```

*   这里我们显示了基于分数的消息。
*   我们的节目已经准备好了，请验证你的爱情量表得分，玩得开心