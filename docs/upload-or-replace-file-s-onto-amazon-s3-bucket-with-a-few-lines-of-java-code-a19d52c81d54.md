# 用几行 Java 代码将文件上传或替换到亚马逊 S3 桶

> 原文：<https://medium.com/geekculture/upload-or-replace-file-s-onto-amazon-s3-bucket-with-a-few-lines-of-java-code-a19d52c81d54?source=collection_archive---------3----------------------->

完整的代码实现+用例演示。

# 背景和用例描述

1 我真正喜欢的亚马逊网络服务(AWS)的主要特性之一是它的[亚马逊 S3 桶——云对象存储](https://aws.amazon.com/s3/),因为它具有多种多样的用例。大约一个月前，我利用这个特性建立了一个静态网站，它需要每天定期更新数据。由于每次数据更新都需要每天替换一个更新的`**{JSON}**`数据文件…