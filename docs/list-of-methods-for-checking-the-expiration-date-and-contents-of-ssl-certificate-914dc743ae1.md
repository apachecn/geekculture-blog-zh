# 检查 SSL 证书的到期日期和内容的方法列表

> 原文：<https://medium.com/geekculture/list-of-methods-for-checking-the-expiration-date-and-contents-of-ssl-certificate-914dc743ae1?source=collection_archive---------11----------------------->

![](img/28bdf9eaa31b42c3bc48323e1a865880.png)

Photo by [Markus Winkler](https://unsplash.com/es/@markuswinkler?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ssl?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

收集了用于在命令行上检查 SSL 证书的到期日期和内容的命令。

# 检查 SSL 证书的到期日期

## 检查本地文件

```
root@vagrant:/home/vagrant# **openssl x509 -noout -dates -in domain.crt**
```

## 服务器端确认

*   HTTPS

```
root@vagrant:/home/vagrant# **echo | openssl s_client -connect example.com:443 2> /dev/null | openssl x509 -noout -enddate | cut -d= -f2**
```

*   简单邮件传输协议

```
root@vagrant:/home/vagrant# **openssl s_client -connect smtp.freesmtpservers.com:25 -starttls smtp | openssl x509 -noout -dates**
root@vagrant:/home/vagrant# **openssl s_client -connect example.com:587 -starttls smtp | openssl x509 -noout -dates**
```

# 验证 SSL 证书的可分辨名称

```
root@vagrant:/home/vagrant# **openssl x509 -noout -subject -in domain.crt**#ORroot@vagrant:/home/vagrant# **openssl x509 -noout -issuer -in domain.crt**
```

# 验证证书文件

```
root@vagrant:/home/vagrant# **openssl x509 -noout -text -in domain.crt**#ORroot@vagrant:/home/vagrant# **openssl asn1parse -in domain.crt**
```

# 检查私钥文件

```
root@vagrant:/home/vagrant# **openssl rsa -noout -text -in domain.key**
```

# 检查 CSR 文件

```
root@vagrant:/home/vagrant# **openssl req -noout -text -in domain.csr**
```

# 检查服务器的 SSL 证书

```
root@vagrant:/home/vagrant# **openssl s_client -showcerts -connect example.org:443**
```

# 检查 SSL 证书的操作

```
root@vagrant:/home/vagrant# **openssl s_server -accept 10443 -cert domain.crt -key domain.key -CAfile domain.ica -WWW**
root@vagrant:/home/vagrant# **openssl s_client -connect localhost:10433**
```