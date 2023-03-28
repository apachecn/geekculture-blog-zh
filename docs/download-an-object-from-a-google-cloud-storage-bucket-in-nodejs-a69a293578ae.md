# 从 NodeJS 中的 Google 云存储桶下载一个对象

> 原文：<https://medium.com/geekculture/download-an-object-from-a-google-cloud-storage-bucket-in-nodejs-a69a293578ae?source=collection_archive---------1----------------------->

![](img/d6ef78ccc586611fb3bdf5a0b5f2cc2e.png)

Photo by [Tony Mucci](https://unsplash.com/@eklect?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 概述

***谷歌云平台*** ( *GCP* )是来自母体*谷歌*的云计算服务套件，是*亚马逊 Web 服务* ( *AWS* )和*微软 Azure* 的劲敌。

我和 GCP 玩了一会儿，因为我正在做一个有趣的金融科技项目，而公司需要可伸缩性。)在一个类似谷歌的环境中。

谷歌云平台配有简单的用户界面和合理的价格。

但是 ***GCP* 的文档有 2 个问题**:

*   有时令人困惑；
*   它在*谷歌*上没有被很好地索引(是的，你理解正确——搜索引擎的*之王*没有正确地索引它自己的文档)。

我不得不从一个*谷歌云存储**GCP*的存储服务】**桶**中**下载 *NodeJS* 中的一些对象，所以我面临一些*摩擦*，我想用这个小指南来帮助你(也帮助我，以后阅读这篇文章)。**

## 谷歌云存储客户端库

为了从*谷歌云存储*桶中下载一个对象，你需要有*谷歌云存储*库。

如果还没有完成，你需要安装*谷歌云存储客户端库*和 *npm* 包管理器:

`npm install @google-cloud/storage`

## 证明

**客户端认证基于一个*服务账户密钥*** ，基本上是一个 *JSON* 密钥。
如果你没有任何键，你可以通过[链接](https://console.cloud.google.com/apis/credentials/serviceaccountkey)创建一个。
关于密匙生成的更多细节，请参见*谷歌文档*此处。

一旦您下载了包含*服务账户密钥*的 *JSON* 文件，建议将其重命名(例如*my-Service-Account-Key . JSON*)并将其移动到适当的文件夹(一个以后不太可能被重命名或删除的文件夹)。

**设置环境变量**

下载的*服务账户密钥*必须设置为环境变量。

**> Linux/MacOS**

```
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/my-service-account-key.json"
```

**>视窗**

记住*Windows**Powershell*和*命令提示符*使用反斜杠`\`来描述路径。

> >【选项 A】*Windows Powershell*

```
$env:GOOGLE_APPLICATION_CREDENTIALS="C:\path\to\my-service-account-key.json"
```

> >【选项 B】*Windows 命令提示符*

```
set GOOGLE_APPLICATION_CREDENTIALS=C:\path\to\my-service-account-key.json
```

> >【选项 C】*Windows Linux 子系统* ( *WSL* )

如果您使用 *WSL* ，也请在 *Linux* 中配置环境变量，否则您将无法通过 *Linux CLI* 运行代码(只有使用 *Windows 命令提示符*才能运行)。

建议将*服务账号密钥*复制到 Linux 文件夹`/etc/profile.d/`中。

然后执行以下命令添加环境变量:

`export GOOGLE_APPLICATION_CREDENTIALS="/path/to/my-service-account-key.json"`

## 从 GCS 下载一个对象，这是一个工作示例

使用下面的代码从 *Google 云存储桶*中下载一个对象，用正确的值更改引用`bucketName`、`srcFilename`和`destFilename`。

**注 1:对象名称**

请注意 ***谷歌云存储*通过一个也包含路径**的名称来标识一个*对象*。
所以，`srcFilename`永远不应该以斜杠`/`开头，因为 *Google 云存储客户端库*会自动将`srcFilename`转换成对象名。

例如，下面的`srcFilename`

`const srcFilename = 'path/to/object/to/download/file.format';`

会被*谷歌云存储*这样解读

`[https://storage.googleapis.com/storage/v1/b/bucket-name/o/path/to/object/to/download/file.format](https://storage.googleapis.com/storage/v1/b/bucket-name/o/path/to/object/to/download/file.format)`

其中`b`标识*桶*名称，`o`标识对象名称。

但是如果在`srcFilename`前面有斜线`/`

`const srcFilename = '/path/to/object/to/download/file.format';`

那么该名称将被转换成

`[https://storage.googleapis.com/storage/v1/b/bucket-name/o//path/to/object/to/download/file.format](https://storage.googleapis.com/storage/v1/b/bucket-name/o//path/to/object/to/download/file.format)`

导致一个错误

`404 Error: No such object: bucket-name//path/to/object/to/download/file.format`

**注 2:目的文件夹**

如果你想把下载的*对象*放到一个特定的文件夹，记得在把值传递给`destFilename`之前**创建目录。**

**运行代码！**

使用默认的`node`命令测试代码:

`node example-code.js`

🙏感谢您的阅读！

🔎你可以在[*Twitter*](https://twitter.com/RapacciniM)*和 [*LinkedIn*](https://www.linkedin.com/in/marco-rapaccini/?locale=en_US) 上找到我。*