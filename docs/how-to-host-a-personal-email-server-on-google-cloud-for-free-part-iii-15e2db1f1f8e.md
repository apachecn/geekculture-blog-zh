# 如何在 Google Cloud 上托管个人邮件服务器(免费！):第三部分

> 原文：<https://medium.com/geekculture/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-iii-15e2db1f1f8e?source=collection_archive---------4----------------------->

## 配置 Dovecot 和加密

![](img/6b2e8f16b70af07c66f9c8ac6b6afd8e.png)![](img/1bbe2cfa654c0305e6c77c0e8448e704.png)

## 关于彼得·艾克斯莱的一个笔记

这篇文章使用*让我们加密*来保护我们的电子邮件&网络流量。“让我们加密”的创始人之一，澳大利亚计算机科学家彼得·艾克斯莱于 2022 年 9 月 2 日去世，享年 43 岁。彼得·艾克斯莱在与我们共事的短暂时间里完成了许多事情，但也许最值得一提的是，他让互联网隐私变得容易负担得起，为更广泛的采用铺平了道路。他是互联网隐私、网络中立和人工智能伦理的倡导者。我只是想花一点时间来认识他对互联网隐私的巨大贡献，因为没有他就不可能有这个指南。请花点时间在这里了解更多他对这个领域的贡献[。](https://community.letsencrypt.org/t/peter-eckersley-may-his-memory-be-a-blessing/183854)

# 本系列文章

1.  [简介& GCP 设置](https://lp3.medium.com/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-i-8124d65d1d25)
2.  [配置后缀、邮件枪、& DNS 记录](https://lp3.medium.com/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-ii-20aaeb0ae9eb)
3.  ***配置鸽笼&加密***
4.  [使用 MariaDB & Postfixadmin](https://lp3.medium.com/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-iv-1b5142cab9c) 管理虚拟邮箱
5.  [使用 Roundcube 托管网络邮件](https://lp3.medium.com/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-v-f9a4b3643622)
6.  [用 Rspamd &筛子过滤垃圾邮件](/geekculture/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-vi-6ea09f18d7df)

如果您还没有阅读本系列的前几篇文章，请按照上面的链接进行阅读。现在，我们已经将运行在 GCP 上的服务器配置为通过 SMTP 协议发送和接收电子邮件，并配置了我们的 DNS 记录，以确保电子邮件发送到我们这里和从我们这里发出。现在，我们需要确保可以从邮件客户端连接到我们的服务器，以检查我们的电子邮件并提交要发送的新邮件。

# 保护我们的电子邮件流量

我们应该做的第一件事是通过获得 TLS 证书来保护我们的电子邮件流量。我们将使用 [*让我们免费加密*](https://letsencrypt.org) 来做到这一点。让我们从安装 Let's Encrypt 客户机开始。

```
sudo apt install certbot -y
```

现在，我们将使用`certbot`启动一个临时服务器，以确认我们确实是该域的所有者，并颁发证书。

```
sudo certbot certonly -d *mail.example.com*
```

使用您的邮件子域并选择选项 *1。启动临时网络服务器(独立的)*。你必须输入一个电子邮件地址并回答几个问题才能继续。如果您收到 DNS 错误，请确保*邮件记录*指向正确的地址。如果您最近创建或更新了记录，请等待几分钟，然后重试。一旦命令成功，就可以在`/etc/letsencrypt/live/*mail.example.com*`找到我们的 TLS 证书。

让我们继续，通过编辑`/etc/postfix/main.cf`，配置 Postfix 使用我们的新证书来实施流量加密。
首先，替换默认的*证书* & *密钥*文件。

```
#TLS parameters
smtpd_tls_cert_file = /etc/letsencrypt/live/*mail.example.com*/fullchain.pem
smtpd_tls_key_file = /etc/letsencrypt/live/*mail.example.com*/privkey.pem
```

我们还想强制 TLS 版本≥ 1.2，因此添加以下几行:

```
#Enforce TLSv1.2 or TLSv1.3
smtpd_tls_mandatory_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1
smtpd_tls_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1
smtp_tls_mandatory_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1
smtp_tls_protocols = !SSLv2, !SSLv3, !TLSv1, !TLSv1.1
```

现在我们的加密已经设置好了，让我们通过 Postfix 启用电子邮件提交。为此，我们将编辑`/etc/postfix/master.cf`。在服务列表中找到`submission`部分，取消对这些行的注释:

```
submission inet n       -       y       -       -       smtpd
  -o syslog_name=postfix/submission
  -o smtpd_tls_security_level=encrypt
  -o smtpd_sasl_auth_enable=yes
# add this value to following following line
  -o smtpd_recipient_restrictions=permit_mynetworks,permit_sasl_authenticated,reject
  -o smtpd_relay_restrictions=permit_sasl_authenticated,reject
```

另外添加以下两行:

```
 -o smtpd_sasl_type=dovecot
  -o smtpd_sasl_path=private/auth
```

之前的设置适用于来自端口 587 上的电子邮件客户端的提交。在`submission`部分的下面是`smtps`部分。它适用于来自端口 465 上的邮件客户端的提交。如果您计划使用需要此端口的电子邮件客户端，如 Microsoft Outlook，也有必要启用此功能。如果是这样，取消注释或添加以下行:

```
smtps     inet  n       -       y       -       -       smtpd
  -o syslog_name=postfix/smtps
  -o smtpd_tls_wrappermode=yes
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_recipient_restrictions=permit_mynetworks,permit_sasl_authenticated,reject
  -o smtpd_relay_restrictions=permit_sasl_authenticated,reject
  -o smtpd_sasl_type=dovecot
  -o smtpd_sasl_path=private/auth
```

现在重启 Postfix 来应用设置。

```
sudo service postfix restart
```

# 搭建鸽棚

我们将使用 *Dovecot* 通过 *IMAP* 与我们的邮箱进行交互。下面将安装 Dovecot 的核心库、IMAP 守护进程和 LMTP 守护进程。后者将用于连接 Postfix 和 Dovecot 以存储邮件(收件箱、已发送邮件、垃圾箱、垃圾邮件等)。

```
sudo apt install dovecot-core dovecot-imapd dovecot-lmtpd -y
```

接下来，我们想要配置 Dovecot 来使用`*Maildir*`格式存储我们的邮件。打开`/etc/dovecot/conf.d/10-mail.conf`并更换`mail_location`。

```
mail_location = maildir:~/Maildir
```

然后我们需要将`dovecot`用户添加到`mail`组，以便 Dovecot 可以读取我们的收件箱。

```
sudo adduser dovecot mail
```

## 使用 Dovecot 将电子邮件发送到邮件存储

现在我们将 Postfix 连接到 Dovecot，让后者将邮件分类并发送到我们的邮件存储器。打开`/etc/dovecot/conf.d/10-master.conf`并更新到`lmtp`服务定义。

```
service lmtp {
 unix_listener /var/spool/postfix/private/dovecot-lmtp {
   mode = 0600
   user = postfix
   group = postfix
  }
}
```

回到`/etc/postfix/main.cf`并告诉 Postfix 使用 Dovecot LMTP 进行邮箱传输。

```
mailbox_transport = lmtp:unix:private/dovecot-lmtp# SMTPUTF8 not supported by Dovecot-LMTP
smtputf8_enable = no
```

## 配置身份验证

接下来，我们需要禁用纯文本身份验证，除非 SSL/TLS。打开`/etc/dovecot/conf.d/10-auth.conf`并取消对该行的注释(靠近文件顶部):

```
disable_plaintext_auth = yes
```

## 配置 TLS 加密

现在我们将配置 Dovecot 来使用我们的 TLS 证书。打开`/etc/dovecot/conf.d/10-ssl.conf`。找到下面的`ssl_cert` & `ssl_key`变量，并将值路径更新到您的证书&密钥。

**注意:**不要漏掉`<`字符。这是必须的。

```
ssl_cert = </etc/letsencrypt/live/*mail.example.com*/fullchain.pem
ssl_key = </etc/letsencrypt/live/*mail.example.com*/privkey.pem
```

就像我们对 Postfix 所做的那样，我们希望确保 Dovecot 使用 TLS 版本≥ 1.2，因此找到`ssl_min_protocol`变量，取消对该行的注释，并将值更新为`TLSv1.2`。

```
ssl_min_protocol = TLSv1.2
```

然后找到`ssl_prefer_server_ciphers`变量，取消对该行的注释，并将值改为`yes`。

```
ssl_prefer_server_ciphers = yes
```

## 配置 SASL 身份验证

接下来，我们需要配置 Dovecot 认证服务器，以便 Postfix 可以使用它。打开`/etc/dovecot/conf.d/10-master.conf`并更新`service auth`部分，取消对以下行的注释:

```
unix_listener /var/spool/postfix/private/auth {
  mode = 0666
}
```

## 自动创建默认文件夹

我们可以配置 Dovecot 在我们的邮箱中自动创建文件夹(已发送邮件、垃圾邮件等)。为此，编辑`/etc/dovecot/conf.d/15-mailboxes.conf`。我们可以在`namespace inbox`部分看到常用文件夹列表。要自动创建文件夹，我们只需在邮箱部分添加`auto = create`。例如:

```
mailbox Junk {
  auto = create
  special_use = \Junk
}
```

您可能希望对`Drafts`、`Junk`、`Trash`和`Sent`文件夹执行此操作。最后，重启 Dovecot & Postfix 让我们所有的修改生效。

```
sudo service dovecot restart && sudo service postfix restart
```

现在检查每个服务的状态。

```
sudo service dovecot status
sudo service postfix status
```

如果两个服务都运行正常，那么到目前为止，您的配置很可能是好的。如果你有任何错误，请仔细检查，你没有错过任何步骤，没有任何打字错误。如果你找不到错误的原因，请随时联系我。

# 结论

咻！我们在这篇文章中讨论了很多，但是我们还没有完全完成。如果我们在自己的电脑上设置这个，我们就不能使用我们的用户名和密码从邮件客户端连接。不幸的是，我们还不能从电子邮件客户端访问我们的服务器，因为我们的用户没有密码。您可能还记得，我们使用 SSH 令牌通过 GCP 进行身份验证。我们将在下一篇文章中通过创建 ***虚拟邮箱*** 来解决这个问题。不过，让我们回顾一下我们在本文中完成了什么。

*   我们使用 Let's Encrypt 通过`certbot`客户端获得一个 TLS 证书。
*   我们为邮件客户端的邮件提交配置了 Postfix。
*   我们安装并配置了 Dovecot 作为我们的 IMAP 服务器软件。
*   我们配置了 Postfix 和 Dovecot 来使用我们的 TLS 证书。
*   我们将 Postfix 连接到 Dovecot 进行身份验证和邮件分类。
*   我们使用 Dovecot 自动创建默认邮箱文件夹。

喘口气，为自己走到这一步而感到欣慰。我们今天完成了很多，但还有更多的事情要做。在下一篇文章中，我们将设置 MariaDB 作为我们的数据库，设置我们的虚拟邮箱，并安装 Postfixadmin 以使邮箱/域管理变得超级简单。

感谢您的阅读！如果你觉得这篇文章很有帮助，并且有兴趣继续关注这个系列的其他部分，请鼓掌并关注即将发布的文章。