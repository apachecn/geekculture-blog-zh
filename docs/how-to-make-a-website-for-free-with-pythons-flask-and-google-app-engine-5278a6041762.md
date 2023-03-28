# 如何用 Python 的 Flask 和 Google App Engine 免费做一个网站

> 原文：<https://medium.com/geekculture/how-to-make-a-website-for-free-with-pythons-flask-and-google-app-engine-5278a6041762?source=collection_archive---------7----------------------->

本教程演示了使用 Python 微框架 Flask 和 Google App Engine 构建和部署网站是多么容易。

**经历**:初学者

## **要求**:

**python 和 html 的基础知识。本教程演示了如何只用 python 和 html 制作一个实时网站。如果你从未使用过 HTML，你可能想从[HTML 和 CSS 初学者指南开始。](https://datasciencecode.medium.com/the-complete-beginners-guide-to-html-and-css-b8ce816fe2ef?)**

**一个 python 解释器比如**[**vs code**](https://code.visualstudio.com/)**或者**[**Spyder**](https://www.anaconda.com/products/individual)**。这些解释器让你很容易编辑你的代码。**

**一个** [**谷歌云账号**](https://console.cloud.google.com/) **和** [**云 SDK**](https://cloud.google.com/sdk/docs/install) **。创建一个帐户是免费的，当你这样做时，你将获得 300 个免费的信用点数。**

![](img/5eccf51edb84657ee78c78255131bec3.png)

Photo by [Chris Ried](https://unsplash.com/@cdr6934?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 设置网站

Google App Engine 需要四个文件来部署 Flask 站点。这些文件是:一个 **HTML 页面**，一个 **Python 文件**作为“控制器】，一个**需求**文件，一个告诉 App Engine 做什么的文件，叫做 **a .yaml** 文件。为这些文件创建一个新目录。除了 HTML 页面之外的所有内容都应该在这个目录中。对于 HTML 页面，创建一个名为“templates”的子目录。

## HTML

对于这个示例站点，我们将使用一个非常基本的 html 页面。将下面的代码复制粘贴到一个名为**index.html**的文件中，并放在“模板”子目录中。

```
<!doctype html>
<html lang=”en”> 
<head> 
<title>Example Flask Website</title> 
<meta charset=”utf-8"> 
<meta name=”viewport” content=”width=device-width, initial-scale=1"></head> 
<body>Hello World!</body> 
</html>
```

## **控制器**

python 文件告诉网站该做什么。我们需要创建一个新的 flask 应用程序，并为我们的 HTML 文件创建一个单独的路径。将以下内容复制并粘贴到一个名为 main.py 的文件中。

```
from flask import Flask, render_template, requestapp=Flask(__name__) @app.route(‘/’)
def root(): 
    return render_template(‘index.html’) if __name__ == ‘__main__’: 
    app.run(host=”localhost”, port=8080, debug=True)
```

此时，我们可以通过运行这个 python 文件并转到 [http://localhost:8080/](http://localhost:8080/) 来查看我们的 html 页面，以确保一切正常。

## Requirements.txt

需求文件应该列出你的网站中使用的所有 python 包。因为我们只使用 Flask，这个文件应该是一行。将以下内容复制并粘贴到一个名为 requirements.txt 的文件中。

```
Flask==2.0.2
```

## app.yaml

app.yaml 告诉 app engine 如何为您的应用提供服务。由于这只是一个例子，我们将保持它的简单，并使用最低要求。将以下内容复制并粘贴到名为 app.yaml 的文件中。

```
runtime: python39
```

# 部署您的网站

在云控制台中创建新项目。打开 Google Cloud SDK shell，将这个项目设置为您的工作项目。

```
gcloud config set project [PROJECT_NAME]
```

现在将当前工作目录设置为存储文件的位置。运行以下命令。

```
gcloud app deploy
```

按照指导完成站点部署，并在为您的 app engine 项目提供的免费域中查看站点。

**恭喜你！您已成功将网站部署到 Google App Engine。请继续关注用 CSS 和 Javascript 定制您的站点！**

在 github [上查看完整的教程代码。](https://github.com/spmcneal/Medium-Flask/tree/main/Flask-AppEngine)