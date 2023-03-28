# 编程中的一些关键术语

> 原文：<https://medium.com/geekculture/difference-between-some-key-jargon-in-programming-d40587e1dc76?source=collection_archive---------1----------------------->

## 编程第一集

## 术语在编程中也很重要

大家好，

这里有一些编程术语似乎总是让程序员感到困惑，比如对象类、Git- Github、Linux -Ubuntu 等等。今天我们将讨论其中一些在软件开发中起主要作用的技术。不再拖延，让我们深入背景。

![](img/e1b06344343a2c0357d40d740cc34822.png)

Photo by [OSPAN ALI](https://unsplash.com/@ospanali?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 目录

*   类别，对象
*   Git，Github
*   哈希、加密
*   最终变量，Java 中的静态变量
*   认证、授权

# 班级

我们可能知道一份建造新大楼的蓝图已经绘制出来了。蓝色将用于建立实际的建筑。同样的想法也用于编程。类是对象的蓝图。这意味着一个类允许我们创建一个特定类的实际实例。属性和方法是在类下定义的。属性用于定义类实例的状态，而方法是类的行为。许多方法和属性可以包含在一个类中。在编程中，使用名为构造函数的方法来创建该类的实例。构造函数通常用与类名相同的名称来定义。根据用途，一个类可能有许多不同的构造函数。同一个程序中可以定义多个类。

Sample class human

# 目标

对象表示该类的实际实例。对象是通过类中定义的构造函数方法创建的。可以创建来自同一个类的多个对象。对象状态是在对象初始化时或程序运行时通过属性值定义的。在一个类下定义的方法可以在从该特定类创建的对象之上执行。

Sample object creation

# 饭桶

根据 [Atlassian Bitbucket 的说法，](https://www.atlassian.com/git/tutorials/what-is-version-control)版本控制是跟踪和管理软件代码变更的实践。版本控制系统是帮助软件团队管理源代码随时间变化的软件工具。版本控制系统帮助软件开发团队更快更聪明地工作。Git 是一个众所周知的用于软件开发的版本控制系统。还有其他几个版本控制系统，如 Mercurial、Subversion 等。

![](img/226602f232ff14232660a8e1452816d9.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 开源代码库

GitHub 是使用 Git 托管软件开发和版本控制系统。它提供了 Git 的[分布式版本控制](https://en.wikipedia.org/wiki/Distributed_version_control)和源代码管理功能。除了版本控制，Github 还有一些自己的附加功能。大多数计算机科学专业的学生在他们大学学习的第一或第二年可能会认为 Git 和 Github 是一样的。但是像 Redis 和 Redislabs 是完全不同的。Github 只是一个服务提供公司，而 Git 是一种技术。

![](img/6f11270b213d7c95b4752e4b2a6625a9.png)

Photo by [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 散列法

哈希是一种加密方法，可以将任何形式的数据映射为固定长度的文本字符串。哈希通常是通过一个称为哈希函数的数学函数来完成的。转换后的固定长度数据称为哈希值。任何长度的数据都可以被散列。但是得到的哈希值总是固定长度的。哈希是单向函数。这意味着我们只能对数据进行哈希运算，而不能从任意时间点的哈希值中获取原始数据。反向散列在技术上是可行的，但所需的计算能力使其不可行。

哈希通常用于验证文件或数据没有被修改或更改。哈希的一个常见用例是应用程序中客户机和服务器之间的密码交换。用户密码的哈希版本存储在应用程序的数据库中。当一个用户输入要在应用程序中验证的密码时，会比较密码的哈希版本。如果它们匹配，用户将被认证，否则验证将失败。嵌入在 ATM 卡中的秘密也用散列法来验证。目前有许多常见的哈希算法，如 MD4、MD5、SHA、RIPEMD、WHIRLPOOL 等。

# 加密

![](img/5c9c2bbf4faf51a7ef4da494e2817347.png)

Photo by [Clarissa Watson](https://unsplash.com/@clarephotolover?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

加密意味着以一种只有拥有相应密钥的人才能解读的方式对数据进行编码。加密是一种双向功能。这意味着加密文本能够转换回原始的纯文本。这叫解密。加密中使用的密钥区分哈希和加密。加密有助于为敏感信息提供数据安全性。下面的例子显示了明文和相关的密文。

```
**Plaintext: HelloWorld****Ciphertext: YtreHjCkuq**
```

大多数加密算法使用混淆和扩散技术从明文创建密文。混淆意味着以明文形式重塑信息，这样拦截者就不能轻易提取它。扩散意味着将信息从一段明文广泛地传播到密文上。替代加密算法往往擅长混淆视听。换位加密算法往往擅长扩散。

在密码术中主要使用两种不同类型的加密方法。

1.  对称密钥加密:加密和解密使用相同的密钥。
2.  非对称密钥加密:加密和解密使用两种不同的密钥，称为公钥和私钥。

# 最终变量

一旦该值被分配给最终变量，它就不能被重新分配。如果我们试图改变这个值，就会产生一个编译时错误。

上面代码的输出 id 如下所示。

```
value of Constant: Thenusan
value of Constant: Thenusan
```

# 静态变量

Static 提供了一个无需首先创建类的实例就可以使用的变量。静态类成员与类本身链接，而不是与对象链接。所有类实例共享变量的相同副本。

因为 brandName 变量是静态的，所以无需显式创建自行车对象就可以在其他地方使用它。输出如下所示。

```
Hero_India
```

# 证明

身份验证是验证用户就是他们所声称的那个人的行为。这是任何安全流程的第一步。身份验证确认用户就是他们所说的那个人。身份验证通过密码、生物识别、一次性 pin 或一些应用程序来实现。数据通过身份验证机制中的 ID 令牌移动。在某些情况下，系统希望在授予访问权限之前成功确认多个因素。这种多因素身份验证(MFA)要求通常用于增强安全性。

# 批准

系统安全中的授权是给予用户访问特定资源或功能的许可的过程。该术语通常与访问控制或客户端权限互换使用。在安全环境中，授权必须总是在身份验证之后。在组织的管理员授予用户访问所需资源的权限之前，用户应该首先证明他们的身份是真实的。数据通过授权中的访问令牌移动。

我相信你已经理解了今天讨论的主题。如果您有任何问题或任何澄清，不要犹豫，通过回复部分与我联系。感谢您花费宝贵的时间阅读本文。

![](img/89472c6fccc999f7c5af273d0559fa17.png)

Photo by [Pete Pedroza](https://unsplash.com/@peet818?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编程快乐，干杯…

# 参考

 [## 类和对象的区别是什么？

### 类和对象的区别是什么？详细说明:类和对象是面向对象的基本部分…

www.w3schools.in](https://www.w3schools.in/java-questions-answers/difference-between-classes-objects/#:~:text=A%20class%20is%20a%20blueprint,a%20variable%20of%20the%20class) [](https://nordicapis.com/what-is-the-difference-between-authentication-and-authorization/) [## 认证和授权有什么区别？北欧 APIs |

### 授权和认证通常被认为是可以互换的。然而，这些系统做着非常不同的事情…

nordicapis.com](https://nordicapis.com/what-is-the-difference-between-authentication-and-authorization/)