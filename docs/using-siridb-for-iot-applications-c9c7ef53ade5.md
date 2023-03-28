# 将 SiriDB 用于物联网应用

> 原文：<https://medium.com/geekculture/using-siridb-for-iot-applications-c9c7ef53ade5?source=collection_archive---------12----------------------->

![](img/c7a836c8f0c6b4289a9c4cdabbf34773.png)

物联网长期以来一直是最受欢迎的技术趋势之一，这是有道理的。它提供了许多可能性和优势。这包括监测数据，如家中的温度或空气质量。但是这些数据必须存储在某个地方以备后用。最好是通过使用非常高效的解决方案，因为物联网设备通常没有很多计算能力。一个非常合适的解决方案是 SiriDB。一个超级快速和非常强大的时间序列数据库。

在本帖中，我们将向您展示如何轻松构建一个气象站，使用 SiriDB 作为其所有测量的数据库。对于这个气象站，我们使用了树莓皮(3 型号 B)与 Sense 帽子相结合。

# **先决条件**

**硬件**

*   树莓派
*   感官帽

**软件**

您将需要最新版本的 Raspberry Pi 操作系统，它已经包含以下软件包:

*   Python 3
*   Python 3 的 Sense HAT

如果出于任何原因，您需要手动安装 Sense HAT 包，请在您的 Raspberry Pi 上执行以下命令:

```
$ sudo apt-get install sense-hat
```

# **设置**

现在我们的 Raspberry Pi 已经准备好了，我们需要遵循几个步骤来安装 SiriDB。

首先，安装所需的 SiriDB 依赖项:

```
$ sudo apt install libcleri-dev libpcre2-dev libuv1-dev libyajl-dev uuid-dev
```

下载 SiriDB 的源代码:

```
$ wget [https://github.com/SiriDB/siridb-server/archive/refs/tags/2.0.46.zip](https://github.com/SiriDB/siridb-server/archive/refs/tags/2.0.46.zip`)
```

> **注意**:修改 URL 中的版本，获取最新版本的 SiriDB。

现在解压缩下载的 zip 文件:

```
$ unzip 2.0.46.zip`
```

之后，编译源代码:

```
$ cd siridb-server-2.0.46/Release/
$ make clean
$ make
```

然后安装 SiriDB:

```
$ sudo make install`
```

最后，使用 pip 安装 Python 的 [SiriDB 连接器(推荐)，该连接器将在后面的步骤中使用:](https://pypi.org/project/siridb-connector/)

```
$ pip install siridb-connector
```

# **配置**

太好了！SiriDB 现已安装。但是为了让 SiriDB 在启动时自动启动，需要将下面名为`siridb-server.service`的文件添加到`/etc/systemd/system`文件夹中:

```
[Unit]
Description=SiriDB Server
After=network.target[Service]
Environment=”SIRIDB_HTTP_API_PORT=9020"
ExecStart=/usr/bin/siridb-server
StandardOutput=journal
LimitNOFILE=65535
TimeoutStartSec=10
TimeoutStopSec=300[Install]
WantedBy=multi-user.target
```

> **注意**:必须添加环境变量`SIRIDB_HTTP_API_PORT`，因为如果不设置 HTTP API 端口(或 0)，SiriDB 的 API 服务将不会启动。查看[文档](https://docs.siridb.net/getting_started/configuration/)了解更多关于在 SiriDB 中使用环境变量的信息。

现在我们需要重新加载守护进程，让系统知道新添加的服务:

```
$ sudo systemctl daemon-reload
```

让我们启用我们的服务，以便它在 Raspberry Pi 重启时不会被禁用:

```
$ sudo systemctl enable siridb-server.service
```

现在让我们开始我们的服务:

```
$ sudo systemctl start siridb-server.service
```

SiriDB 服务器现在在后台运行。

就配置而言，唯一要做的就是在 SiriDB 中创建一个数据库。为此，我们使用了 [SiriDB HTTP API](https://docs.siridb.net/connect/http_api/) 。SiriDB 有一个默认的[服务帐户](https://docs.siridb.net/overview/service_account/)，用户名为`sa`，密码为`siri`，我们将使用这个帐户。对于我们的教程，我们只需要第二个精度数据库。我们还为这个数据库选择了一个持续时间为 6 小时的[分片](https://docs.siridb.net/shards/)，因为我们的测量间隔只有几秒钟。有时，您可能希望每小时甚至每天存储一个测量值。在这种情况下，通过使用更大的碎片持续时间，您的数据库会有更好的性能。

在端口 9020 上运行的 SiriDB 服务器上使用 cURL 和基本身份验证创建数据库:

```
$ curl --location --request POST ‘http://localhost:9020/new-database' \
--header ‘Content-Type: application/json’ \
--header ‘Authorization: Basic c2E6c2lyaQ==’ \
--header ‘Content-Type: text/plain’ \
--data-raw ‘{
    “dbname”: “iot”,
    “time_precision”: “s”,
    “buffer_size”: 8192,
    “duration_num”: “6h”,
    “duration_log”: “3d”
}’
```

> **注意**:传递给头参数`Authorization`的值是单词 Basic word 后跟一个空格和一个 base64 编码的字符串`username:password`。有关 SiriDB 中认证的更多信息，请参见[文档](https://docs.siridb.net/connect/http_api/#authentication)。

# **收集数据**

为了收集数据，我们使用 Python 3 和 Sense HAT 包。

下面名为`sense.py`的 Python 脚本包含了使用 Raspberry Pi Sense HAT 收集一些关键测量值并将其写入 SiriDB 所需的最少代码:

将脚本添加到您的 Raspberry Pi 中，并以下列内容开始:

```
$ python sense.py
```

为了验证一切工作正常，您现在应该每 10 秒钟在 Sense Hat 上看到一条消息:

![](img/1292ace9bacfdf249283eefaa43eea46.png)

Image 1, Our weather station running the Python script.

> **注意**:我们的摄像头无法很好地捕捉到来自 LED 矩阵的信息，但它确实显示了正确的温度。(当然，假设你相信我们🙂)

# **可视化**

仅此而已。一些关键测量现在每 10 秒自动收集一次并写入 SiriDB。

然而，如果能从这些数据中获得一些见解，那就更有价值了。这可以使用 Grafana 这样的可视化应用程序来完成，SiriDB 为此制作了一个现成的插件:[Grafana-SiriDB-http-data source](https://github.com/SiriDB/grafana-siridb-http-datasource)。

Grafana 如何与 SiriDB 结合使用在这里解释[。但在下面，您已经可以看到该应用中的测量数据:](https://siridb.com/blog/update-example-grafana-using-siridb-http-api/)

![](img/eb706e6a3e9c10a0aee49ee4b2f6b1d9.png)

Image 2, Collected data visualized in Grafana.

# **结论**

在这篇博客文章中，我们一步一步地展示了为物联网应用部署 SiriDB 是多么容易和有效。现在你已经有足够的信息可以自己开始使用 SiriDB 了！

如果你想了解更多关于 SiriDB 的信息，你可以访问位于 https://docs.siridb.netT4 的官方文档页面。