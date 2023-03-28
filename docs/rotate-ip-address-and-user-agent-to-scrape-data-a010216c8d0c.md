# 轮换 IP 地址和用户代理来收集数据

> 原文：<https://medium.com/geekculture/rotate-ip-address-and-user-agent-to-scrape-data-a010216c8d0c?source=collection_archive---------0----------------------->

当您运行网络爬虫时，如果它在短时间内从相同的 IP 和设备向目标站点发送了太多的请求，目标站点可能会出现 reCAPTCHA，甚至阻塞您的 IP 地址以阻止您抓取数据。

![](img/e0dfeadd63ffd2ef4de1b367a1470188.png)

Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我将向您展示在您的网络爬虫中应用的两种不同方法，以避免使用 Python 时出现此类问题。
1。轮换你的 IP 地址
2。轮换用户代理

## 旋转 IP 地址

您可以为每个请求提供一个代理。如果你一直使用一个特定的 IP 地址，网站可能会检测到它并阻止它。要解决这个问题，您可以轮换您的 IP，并为每个请求发送一个不同的 IP 地址。虽然这将使您的程序有点慢，但可以帮助您避免来自目标网站的阻塞。你可以使用 tor 浏览器，并据此设置 tor 代理。但是这里我们将使用一个叫做 **torpy** 的 python tor 客户端，它不需要你在你的系统中下载 tor 浏览器。该库的 GitHub 链接如下:

[](https://github.com/torpyorg/torpy) [## GitHub - torpyorg/torpy:纯 python Tor 客户端实现

### Tor 协议的纯 python Tor 客户端实现。Torpy 可以用来与 clearnet 主机或…

github.com](https://github.com/torpyorg/torpy) 

您可以使用以下命令安装该库:

```
pip install torpy
```

假设我们想向以下站点发送请求:

```
urls = [ "https://www.google.com", "https://www.facebook.com", "https://www.youtube.com", "https://www.amazon.com", "https://www.reddit.com", "https://www.instagram.com", "https://www.linkedin.com", "https://www.wikipedia.org", "https://www.twitter.com"]
```

所以，我们要写一个函数，用每个 URL 请求启动一个新的会话。然后遍历所有的 URL，并用一个新的会话传递每个 URL。

code snippet 1

我们可以从这个站点【https://httpbin.org/ip
检查我们的 IP 地址，因此，在第 11 行，我们打印会话的 IP 地址。如果我们执行上面的程序，我们将获得每个请求的 IP 地址。在我的例子中，输出如下所示:

```
{'origin': '107.189.7.175'}
{'origin': '185.220.101.162'}
{'origin': '185.220.101.79'}
{'origin': '103.236.201.88'}
{'origin': '185.220.100.242'}
{'origin': '209.141.53.20'}
{'origin': '198.98.62.79'}
{'origin': '184.105.220.24'}
{'origin': '193.218.118.167'}
```

正如你所看到的，每个请求的 IP 地址是不同的。

## 轮换用户代理

如果没有有效的浏览器信息，大多数网站会阻止请求。因此，我们通常在每个请求中以用户代理的形式传递 bowser 信息，如下所示:

```
import requestsUSER_AGENT = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/600.4.10 (KHTML, like Gecko) Version/8.0.4 Safari/600.4.10"HEADERS = {"User-Agent": USER_AGENT}html_content = requests.get(url, headers=HEADERS, timeout=40).text
```

用户代理通常包含应用程序类型、操作系统信息、软件版本等信息。

当您保持用户代理信息不变时，就像上面的代码片段一样，目标站点可以检测到所有的请求(您的程序正在发送的请求)都来自同一个设备。我们可以通过为每个请求发送一个有效的用户代理和不同的代理来伪造该信息。你可以从这个[网站](https://developers.whatismybrowser.com/useragents/explore/)找到很多有效的用户代理信息。

其思想是制作一个有效用户代理的列表，然后针对每个请求随机选择一个用户代理。因此，让我们列出有效的用户代理:

```
import randomAGENT_LIST = ["Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 Safari/537.36","Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:24.0) Gecko/20100101 Firefox/24.0","Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/91.0.4472.114 Safari/537.36","Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_5) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1.1 Safari/605.1.15","Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:77.0) Gecko/20100101 Firefox/77.0"
]USER_AGENT = random.choice(AGENT_LIST)
```

现在，让我们在代码片段 1 中随机化我们的用户代理，其中我们旋转了 IP 地址。因此，下面的程序会根据每个请求更改您的 IP 地址和用户代理。

另一个简单的尝试方法是在每个请求前添加`time.sleep()`以避免 reCAPTCHA 问题，如下所示:

这里，在第 7 行，我们添加了一个`time.sleep()`方法，它选择 1 到 3 之间的一个随机数。

记住，以上所有方法都会让你的网页抓取比平时慢。但是这些有助于避免被目标站点阻止，并绕过 reCAPTCHA 问题。

作者:萨德曼·卡比尔·苏米克

[](https://www.linkedin.com/in/sksoumik/) [## Sadman Kabir Soumik -人工智能工程师- Venturas Ltd | LinkedIn

### 查看 Sadman Kabir Soumik 在全球最大的职业社区 LinkedIn 上的个人资料。萨德曼·卡比尔有两份工作…

www.linkedin.com](https://www.linkedin.com/in/sksoumik/)