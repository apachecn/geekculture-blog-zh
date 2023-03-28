# 在 AWS 上构建反向代理

> 原文：<https://medium.com/geekculture/building-a-reverse-proxy-on-aws-b8599a21cd5c?source=collection_archive---------3----------------------->

## 求解单个域的两个端点

![](img/429ad9cf6fef1e0de53464ec09bf6fe3.png)

Image by Pixabay via Pexels CC0

# 问题

PayPal 用于向特定地址([https://www.somedomain.com/?page_id=7100](https://www.somedomain.com/?page_id=7100))发送 IPN 消息的订阅。该地址必须处理传入的 post 请求并更新数据库。