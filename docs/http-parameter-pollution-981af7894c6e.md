# HTTP 参数污染

> 原文：<https://medium.com/geekculture/http-parameter-pollution-981af7894c6e?source=collection_archive---------7----------------------->

## 参数篡改方法

![](img/fe343e45ef4b06f6b794d978490024c6.png)

# 什么是 HPP？

是一个 web 应用程序漏洞，通过在现有参数中注入编码的查询字符串分隔符来利用。如果用户输入未被 web 应用程序正确过滤，就会出现此漏洞。当目标系统接受多个同名的参数，并以一种可能不安全或…