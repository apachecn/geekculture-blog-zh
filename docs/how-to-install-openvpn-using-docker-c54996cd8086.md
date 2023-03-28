# 如何使用 Docker 安装 OpenVPN

> 原文：<https://medium.com/geekculture/how-to-install-openvpn-using-docker-c54996cd8086?source=collection_archive---------0----------------------->

![](img/eb86bb289c2bfe120a0b2f4e7dbe9536.png)

在今天这个时代，随着限制的施加，有一个使用 VPN 服务器的巨大需求。有许多免费和付费的 VPN 提供商，但这些提供商也需要一定程度的信任，有时看起来也不可靠。

那么，有一个更可靠、更可控的选择:为什么不托管自己的 VPN 服务器呢！你可能认为这很难做到。但是随着容器化技术的发展，比如今天的 Docker 和一个可爱的社区，它可能会弥补这个差距，并使它更容易在几分钟内完成。

在本文中，我们将看到使用 Kyle Manna 准备的 docker 文件来开始使用自己的 OpenVPN 服务器是多么容易。哦！我差点忘了你需要一个安装 docker 的服务器来开始。我会推荐你选择的任何一家供应商使用云。

首先，让我们使用 git 来克隆存储库

```
git clone [https://github.com/kylemanna/docker-openvpn.git](https://github.com/kylemanna/docker-openvpn.git)
```

我们可以更改目录并构建映像

```
cd docker-openvpn && docker built -t open-vpn-server .
```

然后，我们将创建一个卷映射目录来存储配置文件。

```
mkdir vpn-data && touch vpn-data/vars
```

我们现在将使用 OpenVPN 服务器来生成配置文件

```
docker run -v $PWD/vpn-data:/etc/openvpn --rm open-vpn-server ovpn_genconfig -u udp://IP_ADDRESS:3000
```

在上面的命令中，添加服务器 IP 地址来代替 IP_ADDRESS。

现在，我们将设置我们的密钥。我们将生成 CA 证书和私钥。我们将被要求输入保护私钥的密码。

```
docker run -v $PWD/vpn-data:/etc/openvpn --rm -it open-vpn-server ovpn_initpki
```

现在，我们将利用配置文件运行 VPN 服务器

```
docker run -v $PWD/vpn-data:/etc/openvpn -d -p 3000:1194/udp --cap-add=NET_ADMIN open-vpn-server
```

我们还没有创建任何用户，我们可以创建多个用户来使用我们新设置的 OpenVPN 服务器。我们现在将创建第一个使用服务器的用户

```
docker run -v $PWD/vpn-data:/etc/openvpn --rm -it open-vpn-server easyrsa build-client-full firstuser nopass
```

我们将用户称为“firstuser ”,并使用 nopass 选项禁用了密码验证。如果需要设置密码，可以删除 nopass 标志。

我们还将生成一个配置文件，我们将把它发送给用户以建立到 VPN 服务器的连接

```
docker run -v $PWD/vpn-data:/etc/openvpn --rm open-vpn-server ovpn_getclient firstuser > firstuser.ovpn
```

然后，您可以使用这个文件，通过 OpenVPN Connect 应用程序建立到 VPN 服务器的连接，OpenVPN Connect 应用程序在大多数平台上都可用。

是的，就这么简单。😉