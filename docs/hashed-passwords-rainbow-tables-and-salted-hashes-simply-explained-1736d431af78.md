# 简单解释了散列密码、彩虹表和加盐散列

> 原文：<https://medium.com/geekculture/hashed-passwords-rainbow-tables-and-salted-hashes-simply-explained-1736d431af78?source=collection_archive---------2----------------------->

## 将密码正确安全地存储在数据库中可以避免安全噩梦，但许多组织没有遵循最佳实践并为此付出了代价。

![](img/791b97474a01cfe0fe57d06a9f0a45a6.png)

将密码明文存储在组织的数据库中是一个糟糕的主意。数据库漏洞太常见了，如果您组织的数据库被攻破，攻击者现在可以访问一些(如果不是全部)用户密码。明文仅仅意味着数据以清晰、可读的格式存储，比如“MyPassword1992”。直到今天，许多组织仍然以明文形式存储您的密码，或者是有意的，或者是由于对部分或全部用户的疏忽。就在 2019 年，甚至谷歌和脸书这样的公司也因糟糕的安全实践暴露了用户凭据。你可能认为在现代网络发展中扮演重要角色的公司会更加重视他们自己用户群的安全，但这并不正确。来自脸书最近的一次违约声明:

> “我们估计，我们将通知数亿脸书 Lite 用户，数千万其他脸书用户，以及数万 Instagram 用户。”

这些数字令人震惊。以下是谷歌内部发现的一个漏洞的片段，2019 年[宣布](https://cloud.google.com/blog/products/g-suite/notifying-administrators-about-unhashed-password-storage):

> 我们在 2005 年实现这个功能时犯了一个错误:管理控制台存储了一个未散列密码的副本。这种做法不符合我们的标准。

请特别注意日期。这个漏洞是谷歌 14 年才发现的。尽管谷歌声称没有证据表明密码被未经授权的人获取，但也没有办法确定。在互联网的生命中，14 年是永恒的。

散列密码简单地说就是将明文通过散列函数算法，该算法将明文转换成看起来是乱码的东西，取前面提到的“MyPassword1992”并将其转换成“a0e 01 f 00 D4 d 7 a 17 ea 9d 0240795 e 257 be 25533489”。与加密不同，哈希函数是单向操作，这意味着它不能像加密的内容那样通过正确的密钥解密。如果给定上述散列密码，就没有办法从散列版本中导出“MyPassword1992”。然而，散列的问题是，当通过相同的散列函数算法时，相同的输入将总是导致完全相同的输出。然后，攻击者可以使用一种叫做[彩虹表](https://en.wikipedia.org/wiki/Rainbow_table)的东西，将普通密码与其对应的哈希进行匹配。这节省了攻击者的时间和计算资源，因为他们不再需要等待通过散列函数来运行常用密码列表，然后再尝试侵入帐户。与明文相比，在组织的数据库中存储散列密码是朝着正确方向迈出的一步。这样，如果出现漏洞，攻击者必须做一些工作来确定是否有任何哈希对应于常用的密码。如果在表中找到匹配，就好像密码是以明文存储的一样。彩虹表可以包含数百万个密码，使用常见单词、数字、特殊字符和短语的组合。

用户使用弱密码和可预测密码的习惯太普遍了，再加上组织将密码存储在哈希表(或者，上帝保佑，明文)中，这两者结合在一起，就产生了彩虹表漏洞。解决方案是使用一种叫做[盐](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/)的东西，在通过哈希函数运行密码之前，在密码的开头或结尾添加一个固定长度的随机字符串。这实际上破坏了彩虹表的效用，使“MyPassword1992”看起来像“MyPassword1992h#6gHgd5！”这使得产生的散列完全不可预测，即使用户密码是。如果组织在其数据库中存储这个加盐哈希而不是您的明文密码，当您尝试登录时，它将获取您的密码，加盐并哈希它，并将该值与存储的加盐哈希密码进行比较，而不是将明文与明文进行比较。这样，如果数据库遭到破坏，就没有办法将哈希与已知密码的彩虹表进行匹配，被盗数据也就没有用了。

重要的是要明白，尽管一些组织已经实现了这一附加的安全特性，但是使用强密码仍然是至关重要的。如果有人猜测您的密码，或使用暴力攻击，他们猜测使用普通密码，他们仍然能够危及您的帐户。散列和加盐密码保护您的密码，虽然它存储在组织的服务器上，但它不能保护您的密码不被猜测，不被您桌上的便利贴或您计算机上的明文文件发现。最佳实践包括使用安全的密码管理器，如 [Bitwarden](https://bitwarden.com/) (它本身应该使用一个你能记住的单个、长而复杂的密码来保护，但这是你必须记住的唯一*的*密码)；[2FA](https://authy.com/what-is-2fa/)(双因素身份验证)，如果组织有类似 [Authy](https://authy.com/) 的应用程序(如果组织有该功能的话(如果你能原谅这种表达，任何有价值的组织都应该已经在使用该功能)，以及数据库级的加盐散列密码存储。作为一个用户，你应该绝对控制这个最佳实践列表中的第一个，所以利用好的密码管理器，用你能记住的可靠密码来保护它。2FA 越来越受欢迎，所以检查你的帐户是否有这个选项，如果有就启用它。咸杂碎？让我们希望存储您的证书的组织已经明白了！

参考资料和其他资源:

*   [脸书安全漏洞](https://about.fb.com/news/2019/03/keeping-passwords-secure/)
*   [谷歌安全漏洞](https://cloud.google.com/blog/products/g-suite/notifying-administrators-about-unhashed-password-storage)
*   [彩虹表](https://en.wikipedia.org/wiki/Rainbow_table)
*   [往杂碎里加盐](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/)
*   [密码存储备忘单](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#Use_a_cryptographically_strong_credential-specific_salt)
*   [Bitwarden 密码管理器](https://bitwarden.com/)
*   [什么是 2FA？](https://authy.com/what-is-2fa/)
*   [Authy](https://authy.com/)