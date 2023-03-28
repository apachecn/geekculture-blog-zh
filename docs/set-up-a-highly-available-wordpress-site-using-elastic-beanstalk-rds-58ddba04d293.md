# 使用 Elastic Beanstalk + RDS 建立一个高度可用的 WordPress 站点

> 原文：<https://medium.com/geekculture/set-up-a-highly-available-wordpress-site-using-elastic-beanstalk-rds-58ddba04d293?source=collection_archive---------15----------------------->

![](img/917e9a2c5696d5ab7a2c78626255d91c.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/web?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当你意识到如何在 AWS 上设置 WordPress 就像在迷宫中导航时，你可能会发现自己在这里。它需要将 AWS 文档页面和数百个堆栈溢出建议拼凑在一起。为了给你和我未来的自己节省一些时间，下面的说明将帮助你在 AWS 上建立一个高可用性的 WordPress 站点。

我使用这个设置来托管一个虚拟活动网站，它需要一个基于 WordPress 为 150，000+并发用户构建的登录的、不可缓存的体验。(我们估计，在为期两天的会议结束时，超过 140 个国家的 1，000，000 人参与了此次活动。🎉)

在负载测试期间，我发现数据库是主要的瓶颈。在事件开始之前，我将数据库大小增加到 r6.large16x(有点过分，但为了安全起见)，并将 m5.large 实例的最小数量增加到 60。使用这些网站大小作为基础，结合 EBS 自动缩放，当我们的用户开始同时注册、登录和参与网站时，网站保持运行。

所以，如果你想知道是否有可能在 WordPress 上运行一个高流量、关键任务的网站…它是！下面的步骤概括了如何实现这一点。

# **设置弹性豆茎**

1.  使用此链接启动一个 EBS 应用:[https://console . AWS . Amazon . com/elastic beanstalk/home #/new application？环境类型=负载平衡](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?environmentType=LoadBalanced)
2.  为以下字段输入您想要的任何值:
    -应用程序名称
    -环境名称
    -域(可选择留空)
3.  对于**平台**，选择 PHP。对于**平台分支**，在撰写本文时，*运行在 64 位亚马逊 Linux 2* 上的 PHP 8.0 是推荐和支持的选项。将**平台版本**保留为推荐版本。
4.  对于**应用代码**，选择*样本应用*。
5.  选择**查看并启动**。
6.  查看其余可用的 EBS 选项。
7.  选择**创建 app** 。
8.  一旦创建了环境，点击**配置**并编辑**容量**设置以匹配以下内容。
    -增加**实例类型**(我一般选择 t3.large 或者 m5.large)。该设置可以随时更新。
    -编辑**缩放触发器**部分以匹配以下内容: **度量:**CPU 利用率
    **统计:**平均值
    **单位:**百分比
    **周期:** 1 分钟
    **违反持续时间:** 1 分钟
    **上限:**10%
    **向上缩放增量:** 
9.  编辑**软件**设置并将**内存限制**设置为 *2G* 。
10.  编辑**滚动更新和部署**设置。至少将**部署策略**更改为*滚动*(我喜欢将批量设置为“固定— 2”)，并将**滚动更新类型**更改为基于健康的*滚动。*

# 设置 RDS

1.  使用此链接启动数据库:[https://console . AWS . Amazon . com/rds/home # launch-db instance:gdb = false；s3-import=false](https://console.aws.amazon.com/rds/home#launch-dbinstance:gdb=false;s3-import=false)
2.  对于**引擎类型**，选择*亚马逊极光(版本:兼容 MySQL 的亚马逊极光)*。
3.  在**数据库集群标识符**设置中输入数据库的名称。
4.  编辑**主用户名**和**主密码**并记下这些设置，因为您稍后会用到它们。
5.  将 **DB 实例类**设置编辑为可以处理您的工作负载的大小(我通常从 r6 large 开始，并使用负载测试来确定适合高容量时间的大小)。
6.  在**多 AZ 部署**下，选择*在不同的 AZ 中创建 Aurora 副本或阅读器节点(推荐用于扩展可用性)*。
7.  展开**附加配置**，设置**初始数据库名称**。
8.  选择**创建数据库**。

# 设置 WordPress

1.下载并解压 WordPress 的最新版本

```
~$ **curl** [**https://wordpress.org/wordpress-4.9.5.tar.gz**](https://wordpress.org/wordpress-4.9.5.tar.gz) **-o wordpress.tar.gz**
~$ **tar -xvf wordpress.tar.gz**
~$ **mv wordpress wordpress-beanstalk** ~$ **cd wordpress-beanstalk**
```

2.创建一个特定于 WordPress 的 nginx 配置文件

```
(from the root wordpress-beanstalk directory)
~$ **nano .platform/nginx/conf.d/elasticbeanstalk/nginx.conf****Paste in the following content:**location = /favicon.ico {
  log_not_found off;
  access_log off;
}location = /robots.txt {
  allow all;
  log_not_found off;
  access_log off;
}location / {
  index index.php index.html index.htm;
  try_files $uri $uri/ /index.php?$args;
}location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
  expires max;
  log_not_found off;
}
```

3.建立你的 wp-config.php 档案

```
(from the root wordpress-beanstalk directory)
~$ **nano wp-config.php****Paste in the following content:**<?php
define('DB_NAME', $_SERVER['RDS_DB_NAME']);
define('DB_USER', $_SERVER['RDS_USERNAME']);
define('DB_PASSWORD', $_SERVER['RDS_PASSWORD']);
define('DB_HOST', $_SERVER['RDS_HOSTNAME'] . ':' . $_SERVER['RDS_PORT']);
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');
define('AUTH_KEY',         $_SERVER['AUTH_KEY']);
define('SECURE_AUTH_KEY',  $_SERVER['SECURE_AUTH_KEY']);
define('LOGGED_IN_KEY',    $_SERVER['LOGGED_IN_KEY']);
define('NONCE_KEY',        $_SERVER['NONCE_KEY']);
define('AUTH_SALT',        $_SERVER['AUTH_SALT']);
define('SECURE_AUTH_SALT', $_SERVER['SECURE_AUTH_SALT']);
define('LOGGED_IN_SALT',   $_SERVER['LOGGED_IN_SALT']);
define('NONCE_SALT',       $_SERVER['NONCE_SALT']);
$table_prefix  = 'wp_';
define('WP_DEBUG', false);
if ( !defined('ABSPATH') )
  define('ABSPATH', dirname(__FILE__) . '/');
require_once(ABSPATH . 'wp-settings.php');
```

4.将以下内容添加到您的。gitignore 文件

```
# Elastic Beanstalk Files
.elasticbeanstalk/*
!.elasticbeanstalk/*.cfg.yml
!.elasticbeanstalk/config.yml
!.elasticbeanstalk/*.global.yml
```

# 将 RDS 数据库连接到 EBS

1.  在这里找到您之前创建的 EBS 环境[https://console.aws.amazon.com/elasticbeanstalk](https://console.aws.amazon.com/elasticbeanstalk)(确保您在正确的区域)。
2.  在左侧栏中，选择**配置**，然后在**软件**类别中选择**编辑**。
3.  在**环境属性**部分定义以下变量:
    *要找到所需的值，请导航到您之前创建的 RDS 集群(*[*https://console.aws.amazon.com/rds/home*](https://console.aws.amazon.com/rds/home)*->选择* ***DB 实例*** *- >选择比您创建的读取器和编写器高一级的集群)*

```
RDS_HOSTNAME - On the **Connectivity & security** tab, copy the "Writer" **Endpoint**RDS_PORT - On the **Connectivity & security** tab, copy the "Writer" **Port**RDS_DB_NAME - On the **Configuration** tab, copy the **DB Name**RDS_USERNAME - On the **Configuration** tab, copy the **Master username**RDS_PASSWORD - Cannot be copied from the RDS Console. You made a note of this password when launching the database.For the Salt Keys, I usually use a website like [https://api.wordpress.org/secret-key/1.1/salt/](https://api.wordpress.org/secret-key/1.1/salt/) to generate the following:AUTH_KEY
SECURE_AUTH_KEY
LOGGED_IN_KEY
NONCE_KEY
AUTH_SALT
SECURE_AUTH_SALT
LOGGED_IN_SALT
NONCE_SALT
```

# 在 EBS 上启动你的 WordPress 知识库

你成功了！你终于准备好发布你的 WordPress 网站了！

*先决条件—* [如果您还没有安装 EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html) 。

1.  从您在上面步骤中设置的本地存储库中，在终端中运行 **eb init** 。
2.  在第一步“选择默认区域”中，选择您的 EBS 环境所在的区域。
3.  在“选择要使用的应用程序”步骤中，选择您在上述步骤中创建的应用程序。
4.  在“是否希望继续代码提交？”步骤，按回车键表示不
5.  运行 **eb 部署**。

就是这样！你做到了！