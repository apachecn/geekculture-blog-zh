# 用于并行处理自动气象站 SQS 消息的 Python 脚本

> 原文：<https://medium.com/geekculture/python-script-for-parallel-processing-of-aws-sqs-messages-a6ea60c18f7d?source=collection_archive---------3----------------------->

## 具有最大线程数的 AWS SQS 消息的多线程处理

![](img/20771f9efb56427358c4958d17697a78.png)

Photo by [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/python-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇短文中，我们将学习如何编写一个 Python 脚本，该脚本可以从 SQS 队列接收消息并并行处理它们。我们不能产生无限数量的线程，因为这将导致我们的系统 OOM(内存不足)。还有…