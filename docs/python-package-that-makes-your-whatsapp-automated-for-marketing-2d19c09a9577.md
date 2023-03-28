# Python 包让你的 WhatsApp 自动营销

> 原文：<https://medium.com/geekculture/python-package-that-makes-your-whatsapp-automated-for-marketing-2d19c09a9577?source=collection_archive---------7----------------------->

## AutoWhatsPy 甚至不打开浏览器窗口，在后台工作。而且是开源的。

![](img/0b16f9fa013bb6c9b1f638cee7af202d.png)

WhatsApp. Source: [Pexels](https://www.pexels.com/photo/whatsapp-application-screenshot-46924/).

WhatsApp 是你可以用于营销目的的最佳工具之一。从发送促销信息到定期更新，WhatsApp 可以成为公司或个人可以使用的最伟大的免费工具。

如果你征得客户同意，通过 WhatsApp 信息发送促销信息或新产品或政策变化的更新，这对企业的进一步发展和保持客户参与真的很有帮助。但问题是，一个人必须通过打字或复制粘贴的方式将信息发送给几个联系人。

当你只需要给 5-10 个联系人发信息时，这没什么大不了的。但是，如果你必须给一千个客户发送信息，这项工作听起来几乎不可能手工完成。这就是为什么公司使用电子邮件服务，可以自动发送促销电子邮件和时事通讯到成千上万的电子邮件地址。

但可悲的是，这种服务很贵，一个普通客户几乎不会定期查看邮件。另一方面，一个正常的客户几乎每天都在使用 WhatsApp，在客户同意的情况下，直接向客户的 WhatsApp 收件箱发送简讯确实会有所帮助。

可悲的是，在此之前没有这样的 WhatsApp 自动化服务存在。所以，我做了 AutoWhatsPy。一个简单的 python 包来完全自动化这个过程。

AutoWhatsPy 最好的一点是**它完全在后台工作，不会打开一个烦人的浏览器窗口来工作**。

你可以把一个任务分配给一个用这个包编写的程序，它将在后台工作。从技术上来说，它确实会打开一个浏览器窗口，但不会在前台打开。当电脑在后台运行时，您可以继续在电脑上做其他事情。

你一定在想，“安全吗？”

是的。确实是。另外，我把整个项目开源了，这样你就可以自己看到代码做了什么。这个包完全是使用 selenium 和 Firefox gechodriver 用 python 构建的。

# 装置

打开您的终端并使用 pip 命令安装它。

```
pip install autowhatspy
```

# 用法(针对 WhatsApp)

## *群发 WhatsApp 消息给号码*

通过 WhatsApp 向多个号码发送一条或多条信息。即使这些号码没有保存在您的联系人列表中，这也是有效的。这将工作，即使你从来没有从你的 WhatsApp 信息的号码。这不适用于未在 WhatsApp 上注册或未激活的号码。

```
message_to_numbers(msg, numbers_list, gechodriver, gechodriver_log, user_profile, country_code, image)
```

## 向联系人群发 WhatsApp 消息

通过 WhatsApp 向多个联系人发送一条或多条(或两条)信息

```
message_to_contacts(msg, contacts, gechodriver, gechodriver_log, user_profile, image)
```

## 向群组群发 WhatsApp 消息

通过 WhatsApp 向多个群组发送单个文本或图像(或两者)消息。您必须是拥有发送消息权限的群组成员。

```
message_to_groups(msg, groups, gechodriver, gechodriver_log, user_profile, image)
```

— — —

## 争论

**消息** —包含消息文本的文本文件的路径。

不要在文本中使用不必要的新行，因为它们将作为单独的消息发送给收件人。

**数字列表** —包含数字列表的文本文件的路径。

*文本文件的内容应该是这样的:*

```
919836xxxx10
9624xxxx78
919745xxxx69
7898xxxx56
8985xxxx65
```

这些号码可能有也可能没有国家代码。不管怎样都行。

**联系人** —包含联系人列表的文本文件的路径。

*文本文件的内容应该是这样的:*

```
Samrat Dutta
Abraham Lincoln
Sachin
Sourav
Mr. Das
```

*文本文件中的姓名应与手机中保存的姓名完全相同。你肯定已经在 WhatsApp 上和他们聊过了。*

**组** —包含组列表的文本文件的路径。

*文本文件的内容应该是这样的:*

```
Group One
Group Two
Personal Group
Family Group
Work Group
```

*文本文件中的群组名称应该与 WhatsApp 中的名称完全相同(区分大小写)。您必须是拥有发送消息权限的群组成员。*

**国家代码** —数字的国家代码，无任何符号。示例:91

**gechodriver**—gechodriver.exe 文件的路径。*下载 gechodriver.exe(上面的链接),并将路径作为参数发送。根据解释器的工作方式，您可以使用绝对路径或相对路径。确保 gechodriver.exe 在你正在处理的同一个项目文件夹中。或者在路径中添加 gechodriver.exe 所在的文件夹。否则，可能会发生错误。* *如果还是得到错误，将 selenium 降级到 2.53.6 版本。*

**gechodriver_log** —保存 gechodriver 日志的文本文件路径。*确保 gechodriver.log 位于您正在处理的同一个项目文件夹中。否则，可能会发生错误。*

user_profile —保存的 firefox 用户配置文件的路径。

*   打开火狐。转到“关于:个人资料”并创建新的个人资料。
*   用 firefox 分配给它的任何名字保存在你的项目目录中。
*   打开个人资料，打开 web.whatsapp.com，扫描二维码。
*   将配置文件的路径作为参数发送给函数。
*   图像—(可选)要发送的图像/视频的路径。请确保它受 WhatsApp 支持，并且长度是允许的。

— — —

让我们来看一个示例代码。

# 示例代码

下面是一个示例代码。

```
import autowhatspy
import os

path = path = os.path.dirname(os.path.realpath(__file__)) + "\\"
msg = path + "msg.txt"
numbers_list = path + "numlist.txt"
gechodriver = path + "gechodriver.exe"
gechodriver_log = path + "gecholog.txt"
user_profile = path + "firefoxprofile"
country_code = "91"
image = path + "image.jpg"

autowhatspy.message_to_numbers(msg, numbers_list, gechodriver, gechodriver_log, user_profile, country_code, image)
```

# 结论

AutoWhatsPy 的当前版本是 1.0.7。这个版本运行良好，对我来说，它没有显示任何错误。它生成的输出信息丰富且易于阅读。

我制作这个包是为了帮助小企业进行营销，并向客户发送重要的更新。该软件包也可用于向使用 Gmail 的用户发送批量电子邮件。

## 更多信息:

AutoWhatsPy 文档:[https://pypi.org/project/autowhatspy](https://pypi.org/project/autowhatspy/)

我的 Github:【https://github.com/SamratDuttaOfficial 

我的 LinkedIn:【https://www.linkedin.com/in/SamratDuttaOfficial】T4

请考虑给我的 GitHub 库一颗星。这对我帮助很大。

想雇我吗？在 LinkedIn 上联系我。

最后，请考虑[请我喝杯咖啡](https://www.buymeacoffee.com/SamratDutta)。