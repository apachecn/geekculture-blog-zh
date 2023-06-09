# 在参加编码训练营之前，我应该参加哪些课程？

> 原文：<https://medium.com/geekculture/which-courses-should-i-take-before-a-coding-bootcamp-57fffc024076?source=collection_archive---------5----------------------->

![](img/989340be267e12b46160dd920bad20b8.png)

你被编码训练营录取了，现在怎么办？

你的生活即将改变。跟网飞说再见，跟你的一些私人关系说再见，跟你的薪水说再见，跟几个小时的头撞墙说你好。编码训练营将是一个 12 周的挑战，它将决定你的成败。

你被一个编码训练营录取了，现在你在想，“我刚刚让自己陷入了什么？”

据 Indeed 称，大多数编程训练营的毕业生毕业后会失业 9 个月。

你申请的时候他们可不是这么跟你说的……现在你开始听到那些失业和负债的人的恐怖故事了。你有动力和决心去确保你不是那些人中的一员。你决定制定一个计划开始准备。你上了 YouTube，现在你开始了导致其他人死亡的过程。90%的内容都是错误的建议，会让你长期失业。

那么…正确的建议是什么？

2 年前，我从一个编码训练营毕业，从那以后，我已经帮助 60 多名训练营成员在几周内被录用，被邀请在会议上发言，500 多名训练营成员参加了我的工作准备活动。

有 3 个简单的步骤可以确保你没有负债，也不会走向长期失业。

1.  对课程进行逆向工程
2.  掌握基础知识
3.  关注 20%的概念，这是 80%的编程所基于的

我希望我在去编码训练营之前已经遵循了这三个步骤。我设计了这些步骤，这样你可以只用 24 小时的空闲时间和 40 美元来准备。

# 🛠逆向工程课程

我看到的最大错误是人们在 YouTube 上做与其课程无关的教程。

大多数编码训练营专注于让你成为一名 web 开发人员。他们教你如何使用数据库:MongoDB 或 Postgres。然后如何使用 ORM(通常是 Mongoose 或 Sequelize)并使用 Express 进行基本的路由来查询和修改数据库中的记录。在此之后，您将学习如何使用 React 构建用户界面。

这些都有什么共同点？

节点。Node 是在 web 浏览器之外运行 JavaScript 的运行时环境。如果你觉得最后一句有点过了，不要担心。关键是一切都源于节点和 JavaScript。

你的第一步应该是掌握 JavaScript。

# 📝掌握基础知识

任何应用程序的基础是什么？

每个应用程序都有两样东西:数据和行为。数据的构建块:原始值(字符串、整数和布尔值)和对象(普通的旧 JavaScript 对象、类、数组和函数的实例)。行为的构造块是循环和控制流(if 语句)。

你怎么知道你是否掌握了基础知识？

## 你知道为什么这是假的吗？

```
{} === {}; // false
```

## 你能发现这个漏洞吗？知道这个函数为什么会变异原物体吗？

```
const person = { name: ‘Random Guy’ age: 29, isCool: true, details: { isCopy: false },}; const clonePerson = (person) => { const clone = {…person}; clone.details.isCopy = true; return clone;};
```

一旦你能回答这两个问题，你就准备好继续前进了。

你的下一个目标应该是通过 HackerRank 的这个[基本问题解决认证](https://www.hackerrank.com/skills-verification/problem_solving_basic)。之后，我建议磨出一些更基本的数据结构和算法，代码战争是伟大的，你的目标应该是解决 5 级形的。

你的目标应该是理解基本的数据结构&搜索和排序算法。其他任何东西都太先进了，还不是正确的关注领域。

# 80/20 法则

编程中 20%的概念占据了你日常工作的 80%。

你要学习和掌握的概念是闭包、高阶函数、回调、异步 JavaScript 和面向对象 JavaScript。

最好的方法是参加威尔·森塔尼斯的 [JavaScript 难点](https://frontendmasters.com/courses/javascript-hard-parts-v2/)课程。我个人作为一名中级开发人员参加了这个课程，并从中受益匪浅——学习这些概念将有助于您在受阻时进行沟通。在 coding bootcamp，你的团队中会有 50 名其他学生，你不会得到个性化的帮助，所以成为一名强大的技术交流者并了解 JavaScript 的基础知识将有助于你交流你正在处理的问题。

现在，工作 24 小时，花费 40 美元，你将领先于 80%的同龄人。

如果你在第一天就进入了前 20%的行列，你的工作前景会是怎样的？