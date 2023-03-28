# 如何在 Heroku 上部署 Django 应用程序

> 原文：<https://medium.com/geekculture/how-to-deploy-a-django-app-on-heroku-4d696b458272?source=collection_archive---------0----------------------->

## 一步一步的指南。

![](img/638dc08dfcab1bc1cc228da71a00a538.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是一些开发人员感到失落的时刻:部署时间。一些应该是微不足道的事情，有时不是，但它根本不应该这么难——事实上，它不是。希望在本指南结束时，您能够在 Heroku 上托管您的 Django 项目，Heroku 是一个很好的云服务，尤其是因为 Heroku 是一个云 PaaS(平台即服务),并且还提供了一个免费的计划，您可以使用该计划部署您的项目，并提供完整的数据库供应。

在开始之前，您必须对您的项目进行一些配置，以使它在服务器上工作。开发和生产之间是有区别的，本教程将引导你通过它。

*注意:我假设你使用的是* ***虚拟环境*** *。您还必须创建一个* `***requirements.txt***` *文件，并将运行 web 应用程序所需的所有 python 包(包括 Django 本身)添加到其中。如果您还没有创建这个文件，那么您可以在您的终端上运行下面的命令:*

```
*pip3 freeze > requirements.txt*
```

首先你需要一个`**Procfile**`，用来声明应用进程类型。该文件必须位于项目的根目录中。您可以在终端上运行以下命令来创建一个:

```
*echo "**web: gunicorn PROJECT_NAME.wsgi" > Procfile*
```

其中`**PROJECT_NAME**`是您的 Django 项目的名称。

接下来，您需要安装一些包来使应用程序在服务器上工作。这些是必需的软件包:

*   gunicorn(一个应用服务器)
*   DJ-数据库-url(用于数据库)
*   whitenoise(用于服务静态文件)
*   psycopg2(用于 postgres 数据库)

要一次性安装它们，只需在您的终端上运行:

`*pip3 install gunicorn dj-database-url whitenoise psycopg2-binary*`

安装完成后，将它们添加到您的需求中。

`*pip3 freeze > requirements.txt*`

现在您必须在根目录下创建一个`**runtime.txt**`文件——就像 Procfile 一样——并添加您正在使用的 python 版本。要检查您使用的 python 版本，请运行:

```
*python3 --version*
```

输出将是这样的:

```
*Python 3.8.5*
```

由于我使用的是 **Python 3.8.5** ，为了创建*运行时*文件并添加 Python 版本，我可以运行以下命令:

```
*echo "python-3.8.5" > runtime.txt*
```

你还应该创建一个`**.gitignore**`文件，并在其中添加 *sqlite* *数据库*，只要*虚拟环境文件夹*。要执行此运行，请执行以下操作:

```
echo "db.sqlite3" > .gitignore
echo "env/" >> .gitignore
```

其中`**env/**`是您的虚拟环境的名称。

***注意:第二个命令应该使用两个箭头(> >)，而不是一个箭头(>)，因为你要追加到文件中。使用一个单独的箭头(>)将文件内容替换为新的内容。***

现在，您必须配置一些项目文件，以使应用程序在服务器上工作。首先，转到`**settings.py**`文件并执行以下操作:

*   为了让`**whitenoise**`处理静态文件，您必须在`**INSTALLED_APPS**`的列表中添加下面一行:

*   将以下行添加到`**ALLOWED_HOSTS**`列表中:

*   您还必须将**调试**设置为**假**:

*   现在您必须将`**whitenoise**`添加到`**MIDDLEWARE**`列表中，但这部分比较棘手—****whiten waise 中间件应该包含在*** `***django.middleware.security.SecurityMiddleware***`之后*

*   *现在您必须添加静态文件的配置。在你的`**settings.py**`底部，加上这几行:*

## *最后，但同样重要的是，您必须配置 Postgres 数据库*

*   *在`**DATABASES**`字典之后，添加以下几行:*

*是的，这是很多，但现在你的应用程序已经准备好了。*

## *Heroku 部分*

*现在你必须建立你的 Heroku 账户(如果你没有的话)。这里可以做[。](https://signup.heroku.com/)*

*你还必须安装 Heroku-CLI 。如果您在 Mac 上，您可以安装运行:*

```
**brew install heroku/brew/heroku**
```

*如果您使用的是 Linux (Ubuntu)，请运行:*

```
**sudo snap install heroku --classic**
```

*在 Windows 上，你必须下载安装程序，这可以从[这个页面](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)完成。*

*安装完成后，您必须登录。在您的终端上，运行:*

```
**heroku login**
```

*…并按照说明进行操作。*

*现在，要创建一个新的应用程序，运行:*

```
**heroku create PROJECT_NAME**
```

*其中`**PROJECT_NAME**`是项目的名称。*

*接下来，提交您的更改并推动您的项目部署您的应用程序:*

```
**git add -A
git commit -am "commit message"
git push heroku master**
```

*就这样，恭喜你！您刚刚部署了您的应用。但是您仍然需要配置数据库。要添加 Postgres 附加组件，您必须运行:*

```
**heroku addons:create heroku-postgresql:hobby-dev --app PROJECT_NAME**
```

*其中`**PROJECT_NAME**`是您项目的名称。*

*现在您迁移到数据库:*

```
**heroku run python manage.py migrate**
```

*最后，您必须创建一个超级用户:*

```
**heroku run python manage.py createsuperuser**
```

*如果一切顺利，您已经成功部署了 Django 应用程序。要打开您的应用程序，请运行:*

```
**heroku open**
```

# *全部完成！*

*如果你走到这一步，恭喜你！你不仅将你的项目部署到了 Heroku，还学到了很多新东西——我希望如此。*

*感谢阅读！*