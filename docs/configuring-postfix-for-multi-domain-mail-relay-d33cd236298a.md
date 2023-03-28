# 为多域邮件中继配置 Postfix

> 原文：<https://medium.com/geekculture/configuring-postfix-for-multi-domain-mail-relay-d33cd236298a?source=collection_archive---------0----------------------->

![](img/6b5d4e2762fa89a9ad24f54e573c4aaa.png)

用 Postfix 托管多域电子邮件环境相对简单。好吧，就像管理电子邮件一样简单……设置虚拟邮箱，通过设置管理用户界面( *postfixadmin* )让生活变得更简单，你就大功告成了。但是，如果您在限制端口 25 接收流量的环境中运行服务器，该怎么办呢？这就是谷歌云和其他云服务为防止恶意行为者滥用其云环境发送垃圾邮件而做的事情。

为了解决这个问题，我们必须使用邮件中继服务，比如 [Mailgun](https://www.mailgun.com) 、 [MailJet](https://www.mailjet.com) 或 [SendGrid](https://sendgrid.com) 。这需要一些额外的步骤。在本文中，我们将研究托管多域电子邮件服务器所需的步骤，该服务器使用 postfix 中继服务。这不是一个设置邮件服务器的端到端指南，而是对我之前的系列文章 [**如何在 Google Cloud 上托管个人邮件服务器的补充(免费！)**](/geekculture/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-i-8124d65d1d25) 。假设您拥有使用虚拟邮箱的正确配置的单域电子邮件环境，并且能够正确配置 DNS 以将其他域的电子邮件定向到您的服务器，以及获取其他域的 SSL 证书。如果您的配置不同或者您刚刚开始，请查看之前的指南。

# 后缀配置

实际上，我们只需要配置 2 个更改，但每个更改都包括几个步骤。首先，我们需要为每个域提供中继身份验证参数的映射。然后，我们需要为每个域提供 SSL 证书的映射。如果为所有域颁发了一个证书，后一步是可选的，但是强烈建议每个域都有自己的证书。让我们开始吧。

## 每个域的身份验证

如果您按照指南的第 2 部分中的[操作，您会看到一个`/etc/postfix/sasl_passwd`文件，其中有一行类似于下面的内容(*用户名* & *密码*是您自己的):](/geekculture/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-ii-20aaeb0ae9eb)

```
[smtp.mailgun.org]:2525    username:password
```

您可能在创建地图后删除了该文件，因此如果是这样，您必须重新创建该文件。我们想要做的是更新映射以使用每个域的身份验证。一旦您有了每个域的 SMTP 凭证，更新文件如下:

```
@example.com     username:password
@example2.com    username2:password2
@example3.com    username3:password3
```

**注意:**您的邮件中继服务可能要求每个域使用不同的认证参数，或者对所有域使用相同的认证参数。请遵循您的提供商提供的指南。

然后，使用以下命令从该文件创建地图。

```
sudo postmap /etc/postfix/sasl_passwd
```

现在打开`/etc/postfix/main.cf`添加下面一行。

```
smtp_sender_dependent_authentication = yes
```

这告诉 Postfix 我们需要在依赖于发送者的基础上通过中继服务进行认证。这将关闭 SMTP 连接缓存，以确保每个请求都使用正确的凭据。这就是 SASL 认证。

## 每个域的加密

现在我们将创建一个映射，将 Postfix 指向每个域的 SSL 证书。创建文件`/etc/postfix/ssl_map`并添加以下行，用您自己的路径替换域和证书路径。

```
mail.example.com
    /etc/letsencrypt/live/mail.example.com/privkey.pem
    /etc/letsencrypt/live/mail.example.com/fullchain.pem

mail.example2.com
    /etc/letsencrypt/live/mail.example2.com/privkey.pem
    /etc/letsencrypt/live/mail.example2.com/fullchain.pem

mail.example3.com
    /etc/letsencrypt/live/mail.example3.com/privkey.pem
    /etc/letsencrypt/live/mail.example3.com/fullchain.pem
```

然后，运行以下命令。

```
sudo postmap -F hash:/etc/postfix/ssl_map
```

我们在这里使用`-F`选项，因为我们要映射到文件。

我们还想将这个命令添加到部署后挂钩中，以便在证书更新时运行。更新`/etc/letsencrypt/renewal-hooks/deploy/mail-permissions`像这样:

```
#!/bin/sh
setfacl -R -m u:www-data:rx /etc/letsencrypt/live/ /etc/letsencrypt/archive/
postmap -F hash:/etc/postfix/ssl_map
service dovecot restart
service postfix restart
```

告诉 Postfix 在`/etc/postfix/main.cf`中添加下面一行来使用新地图。

```
tls_server_sni_maps = hash:/etc/postfix/ssl_map
```

然后，重启 Postfix。

```
sudo service postfix restart
```

## 鸽房

我们还必须告诉 Dovecot 为每个域使用正确的证书。打开`/etc/dovecot/conf.d/10-ssl.conf`，在`ssl_cert`和`ssl_key`下为每个域添加一个`local_name`条目。不要注释掉这些默认值。

```
...

ssl_cert =</etc/letsencrypt/live/mail.example.com/fullchain.pem
ssl_key =</etc/letsencrypt/live/mail.example.com/privkey.pem

local_name mail.example.com {
   ssl_cert =</etc/letsencrypt/live/mail.example.com/fullchain.pem
   ssl_key =</etc/letsencrypt/live/mail.example.com/privkey.pem
}

local_name mail.example2.com {
   ssl_cert =</etc/letsencrypt/live/mail.example2.com/fullchain.pem
   ssl_key =</etc/letsencrypt/live/mail.example2.com/privkey.pem
}

...
```

最后重启 Dovecot。

```
sudo service dovecot restart
```

**注意:**如果您按照本系列的[指南](/geekculture/how-to-host-a-personal-email-server-on-google-cloud-for-free-part-v-f9a4b3643622)使用 Roundcube webmail，您可以基于每个域对其进行配置。这不是必须的，但是如果你需要的话会很有帮助。如果你有兴趣的话，请点击这里查看 Roundcube 指南。

# 结论

干得好！我们现在可以通过自己选择的中继服务从服务器上的多个域发送电子邮件。我希望你喜欢这个指南，并发现它很有帮助。如果有，请鼓掌，关注我，跟上我以后的文章和指南。如果您有任何问题或意见，我希望收到您的来信。感谢阅读！