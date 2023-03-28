# 如何使用 Python AWS Lambda 将文件上传到 S3

> 原文：<https://medium.com/geekculture/how-to-upload-file-to-s3-using-python-aws-lambda-9aa03bb2c752?source=collection_archive---------0----------------------->

![](img/27709e26d1edef9a2a49fce087fedec3.png)

Source ([link](https://www.parkmycloud.com/blog/aws-lambda/))

在这篇短文中，我将向你展示如何使用 AWS Lambda 将文件上传到 AWS S3。我们将使用 Python 的`boto3`库将文件上传到 bucket。一旦文件上传到 S3，我们将生成一个预签名的获取 URL，并将其返回给客户端。

如果你想用 React 本地应用上传图片到 S3，请参考这篇文章。