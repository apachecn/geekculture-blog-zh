# 使用 Openssl 安全日志

> 原文：<https://medium.com/geekculture/diy-secure-journal-using-openssl-4263ab96fa1f?source=collection_archive---------19----------------------->

在这篇文章中，我提出了一个简单的数字日志，它可以在所有的操作系统上运行，并且经得起时间的考验。现在有很多写作平台都有云存储支持。使用一种已经存在了 20 多年的工具，来开发一种非常简单、安全且经得起未来考验的方法怎么样？

![](img/2b0fc1b296f61c4604a13b739b57cb98.png)

Journals

Openssl 于 1998 年首次发布，为互联网上使用的代码提供了一套免费的加密工具。从那以后，它被广泛采用，并且在 oss 中无处不在。

下面是一组加密/解密日志的命令。您可以选择使用通用云存储服务(如 Google Drive 或 Dropbox)来备份加密文件。

## 写入和加密:

1.  安装 OpenSSL:如果你还没有它，你可以使用你系统的软件包管理器来安装它。比如 Ubuntu 上的`apt-get install openssl`，macos 上的`brew install openssl`。为了在 Windows 上访问日志，我使用了 [wsl2](https://learn.microsoft.com/en-us/windows/wsl/install) 。
2.  用你最喜欢的文本编辑器写日记。你可以使用任何文本编辑器，如 Nano，Vi，MS Word 等。
3.  一旦建立了日志文本文件，就可以使用 OpenSSL 通过密码对其进行加密。为此，请打开终端窗口并导航到日志文件所在的目录。然后，输入以下命令(参考:Openssl 手册页。).该命令将提示您输入两次密码以确保其正确。

```
openssl aes256 -e -iter 100000 -salt -in journal.txt -out journal.encrypted
```

4.安全删除您的 plantext(未加密)日志文件。在这个例子中，我使用了`shred`命令，将随机比特写到你的明文日志文件中。然后，您可以安全地删除该明文文件。

```
shred journal.docx
rm -f journal.docx
```

5.将加密日志复制到您喜欢的云存储映射文件夹中。

```
cp journal.encrypted /path/to/google/drive
```

**解密和读/写:**

1.  要解密之前加密的日志，请使用以下命令。

```
openssl aes256 -d -iter 100000 -salt -in journal.encrypted -out journal.txt
```

2.读取/更新您的日志，然后使用“写入和加密”一节中的步骤重新加密。

**最终想法**

如果您有图像和其他文件，您可以将它们放在一个目录中，使用实用程序(例如`tar`、`zip`)进行压缩，然后对压缩文件进行加密。我建议将上述命令包装起来，这样就不需要复制/粘贴长的 tar 和 openssl 命令。下面是一个带有两个脚本的回购示例[kaw sark/Journal:DIY OpenSSL Journal(github.com)](https://github.com/kawsark/journal)

*   `push.sh` —将文件加密并复制到备份位置。
*   `pull.sh` —解密文件。

在这篇文章中，我提议使用 Openssl 创建一个简单安全的 DIY 数字日志系统。希望你觉得有用。