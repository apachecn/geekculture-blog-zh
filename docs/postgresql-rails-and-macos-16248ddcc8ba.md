# PostgreSQL、Rails 和 macOS

> 原文：<https://medium.com/geekculture/postgresql-rails-and-macos-16248ddcc8ba?source=collection_archive---------7----------------------->

## 命令行安装教程

## 如何在 Rails 项目中安装和部署本地 Postgres 服务器

![](img/804967fcc41d241d166e246515b10151.png)

Photo by [Jan Antonin Kolar](https://unsplash.com/@jankolar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Rails 安装的默认数据库框架是`sqlite3`。顾名思义，它是一个轻量级的基于 SQL 的数据库系统，开箱即用。不幸的是，Heroku 不支持这个框架，它需要你的代码利用 Postgres 来运行。我认为建立一个本地 Postgres 服务器来熟悉这个框架是个好主意。

# Postgres 安装和设置

安装和管理 Postgres 有多种选择。Postgres.app 是一个 GUI，它还会为您下载并初始化一个 Postgres 服务器。 [PgAdmin](https://www.pgadmin.org/) 自动自带[Postgres 网站](https://www.postgresql.org/download/)提供的安装程序。连接到现有的服务器，提供另一个 GUI。

我选择了命令行安装，因为我想尝试一下，了解如何在没有图形用户界面帮助的情况下与一个基本的 Postgres 服务器进行交互。

## **1。使用** [**自制软件**](https://brew.sh/) 安装 Postgres

```
brew install postgresql
```

使用 Rails 和`sqlite3`，数据库是单独的文件，由 ActiveRecord 直接生成和管理。Postgres 将数据库托管在一个可以打开和关闭的本地服务器上。 你需要自己管理服务器，在连接到 Rails 之前设置用户和数据库。

家酿启动服务器，并在安装时自动创建一个名为`postgres`的默认数据库。您可以通过键入`brew services list`来检查您的服务器状态，这将返回以下输出:

```
Name       Status  User          Plist
postgresql started YOUR_USERNAME /Users/YOUR_USERNAME/Library/Launch
Agents/homebrew.mxcl.postgresql.plist
unbound    stopped
```

家酿将初始化一个用户，并给它当前在 macOS 上登录的任何帐户的用户名*(替换上面例子中的*`*YOUR_USERNAME*`*)*。

要启动和停止服务器:

```
brew services start postgresqlbrew services stop postgresql# You can check the status of your server at any time:brew services list
```

## **2。利用**和`**psql**`

Postgres 的自制安装预打包了`psql`，这是一个用于本地 Postgres 服务器的命令行界面。要进入`psql`环境:

```
psql postgres
```

*边注:调用* `*psql*` *时可以使用任何有效数据库的名称。* `*postgres*` *是家酿初始化的默认数据库，所以我们将用它来启动。*

一些方便的命令可以帮助您随时了解正在发生的事情:

```
postgres=# /* Inside of psql environment */-- list available SQL commands
\help-- list backslash commands
\?-- Display current user
SELECT user;-- Display current database
SELECT current_database();-- Lists existing databases
\l-- Lists users and permissions
\du
```

## 3.创建数据库

```
CREATE DATABASE your_database_name;
```

创建的数据库的所有者将是会话的当前用户。如果愿意，您可以指定不同的所有者，但前提是当前用户拥有必需的权限。

```
-- Create a database and assign its owner
CREATE DATABASE your_database WITH OWNER = different_user;
```

确保为您的项目创建两个数据库。一个用于测试环境，一个用于开发。

```
CREATE DATABASE your_database_name_test;
```

## 4.创建用户

```
-- WITH PASSWORD clause is optional
CREATE USER new_user WITH PASSWORD 'password_goes_here';
```

如果您愿意，可以让您的新用户成为您创建的新数据库的所有者。

```
ALTER DATABASE your_database OWNER = new_user;
```

成功！您的 Postgres 服务器现在应该可以用于您的 Rails 项目了。

# 为 Postgres 配置 Rails

如果从头开始创建一个新的 rails 应用程序，只需使用`--database`标志声明 Postgres。这应该能让你大部分时间到达那里。

```
rails new myapp --database=postgres
```

## 1.红宝石

仔细检查以确保你的 Gemfile 有`pg`宝石。添加`dotenv-rails`来帮助处理数据库凭证。

```
# /Gemfilegem 'pg'
gem 'dotenv-rails'
```

继续之前，请务必运行`bundle install`。

## 2.环境变量

使用`touch .env`创建一个环境文件，一定要在`.gitignore`中列出，防止被推送到 GitHub。然后列出以下信息:

```
# ./envPOSTGRES_USER='new_user'# If you declared a password when creating the database:
POSTGRES_PASSWORD='password'POSTGRES_HOST='localhost'POSTGRES_DB='your_database_name'POSTGRES_TEST_DB='your_database_name_test'
```

## 3.更新 database.yml

现在转到`config/database.yml`并插入您声明的环境变量。

```
# config/database.ymldefault: &default
 adapter: postgresql
  encoding: unicode
  username: <%= ENV['POSTGRES_USER'] %>
  password: <%= ENV['POSTGRES_PASSWORD'] %>
  pool: 5
  timeout: 5000
  host: <%= ENV['POSTGRES_HOST'] %>development:
  <<: *default
  database: <%= ENV['POSTGRES_DB'] %>test:
  <<: *default
  database: <%= ENV['POSTGRES_TEST_DB'] %>production:
  <<: *default
  database: <%= ENV['POSTGRES_DB'] %>
```

完成最后一步后，您应该准备好像平常一样生成迁移了。*感谢阅读，编码快乐！*